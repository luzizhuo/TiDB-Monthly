# 【故障解读】v5.1.2 - TiCDC 不同步，checkpointTs 不推进

## 作者介绍

唐万民,10 多年的数据库运维经验，曾任德邦物流数据库组负责人，长虹数据库架构师，目前担任多点 RDBMS 数据库组负责人，从 TiDB 2.1.6版本接触至目前5.1.4的生产版本，与 TiDB 的发展共同前行，针对踩过的坑提出过很多相关建议。



## 问题现象

### 环境

集群版本：v5.1.2

Tidb server：16c 32g 7台

Ticdc 机器：16c 64g 2台高可用

问题发生版本：v4.0.9  v4.0.13  v5.1.2 

Tidb server 发生oom后，ticdc checkpointTs 不向前推进，尝试pause  changefeed后未恢复，尝试使用tiup重启cdc组件后未恢复。

检查ticdc日志出现大量warning日志：

[2022/01/30 11:17:55.761 +08:00] [WARN] [region_worker.go:377] ["region not receiving resolved event from tikv or resolved ts is not pushing for too long time, try to resolve lock"] [regionID=40982339] [span="[7480000000000016ffa05f72f000000019ff3a9b7b0000000000fa, 7480000000000016ffa05f72f000000019ff7319ad0000000000fa)"] [duration=17m21.65s] [lastEvent=93.273628ms] [resolvedTs=430836713476849684]

[2022/01/30 11:17:55.771 +08:00] [WARN] [region_worker.go:743] ["The resolvedTs is fallen back in kvclient"] ["Event Type"=RESOLVED] [resolvedTs=430836713699672811] [lastResolvedTs=430836984350245604] [regionID=31134532]

TiDB server oom监控

![img](https://asktug.com/uploads/default/original/4X/b/0/b/b0b230685c228b59e2efa6bed04e37928576bbdf.png)

Tidb cdc监控

![img](https://asktug.com/uploads/default/original/4X/2/0/5/205843a7a9d85607f2b4686ec4aa46a6647b56ae.png)



## 问题排查

### TiDB server  oom原因排查

从相关慢查询中

初步排查结果是研发当时查询dashboard慢查询页面导致tidb server发生oom

dashboard的慢查询页面导致oom，从4.0版本到5.1.2版本，曾经多次发生，目前版本还存在该问题，在未来的版本中会得到修复。

目前的缓解方案：

- 尽可能减少 slow-log 文件的数量比如定期做 slow log 的归档删除或设置 log.file.max-days，调大 slow query 的阈值log.slow-threshold。
- 在查询时选择时间范围选小一点，比如一小时以内，并尽量避免多人并发查询 slow query 。
- 查询时尽量不要使用order by排序功能。

![img](https://asktug.com/uploads/default/original/4X/e/8/b/e8bfda7e8ffbf112d499da8036cfd54e729676b4.png)



目前tidb在分析oom问题上提供了一个oom tracker工具，能将当时的top memory sql以及heap统计到tidb server的tmp目录下以供分析，较以前版本来说，排查问题相对简单容易很多。

### TiCDC 问题排查

从监控中可以看到，在tidb server oom后，部分tikv节点中resolved-lag增大

![img](https://asktug.com/uploads/default/original/4X/c/c/0/cc0a0e361cea8be00ef38525a4e5895e406828b2.jpeg)



通过cdc研发人员确认，tidb在事务中对相关表加乐观或悲观锁，当tidb server  oom后，相关锁未正常释放，且region merge会阻塞resolveLock功能，导致lock无法释放



在最新的5.1.4版本中，已经修复了该问题

![img](https://asktug.com/uploads/default/original/4X/0/e/a/0ea6a601299a34a656f54cd570fb59e312e2509c.png)





## 问题处理

### 5.1.4版本及之后版本

tidb server oom后cdc进程checkpointTs 不向前推进问题可以得到解决，不需要特别进行处理。

### 5.1.4之前的版本需要手动处理

##### Workaround：

上一次出现类似问题，使用select count(*) from tb_name，通过对表做count操作来释放相关锁。

也有可能会遇到 count 表后没有恢复的情况。

如果是悲观锁的话，select count 无法解锁，也有可能锁存在于 index 上，select 需要使用 use index，即每一个 index 都 count 一次。

##### 如何判断count后锁是否被释放

当count(*)后启动changefeed进程且cdc log中出现大量"The resolvedTs is fallen back in kvclient"日志时，说明锁未被释放。释放锁之前建议暂停问题链路，问题链路会导致正常链路checkpointTs也不向前推进的情况。



问题修复：kv client 触发 resolved lock 的逻辑有问题，不能及时触发，详见：

https://github.com/pingcap/tiflow/issues/2867



## 相关信息

[fallback resolvedTs event skips the lock resolving unexpectly · Issue #3061 · pingcap/tiflow · GitHub](https://github.com/pingcap/tiflow/issues/3061)

https://github.com/tikv/tikv/pull/11991