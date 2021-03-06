# Summary

[TiDB 社区技术月刊](./readme.md)


- [2022 年 2 月刊](./tidb-202202/prefix-feb.md)
   - [故障解读](./tidb-202202/failure-analysis/failure-analysis.md)
      - [v5.3.0 - BR 备份报错并且耗时比升级前更长(v2)](./tidb-202202/failure-analysis/1-br.md)
      - [v5.1.2 - TiCDC 不同步，checkpointTs 不推进](./tidb-202202/failure-analysis/2-ticdc.md)
      - [v5.1.1 - 调整变量 tidb_isolation_read_engines 影响 tiflash SQL 执行计划](./tidb-202202/failure-analysis/3-tiflash-sql.md)

   - [技术分享](./tidb-202202/usercase/usercase.md)
      - [诊断 SOP | TiKV/TiFlash 下线慢](./tidb-202202/usercase/1-tikv.md)
      - [诊断 SOP | GC 相关问题排查](./tidb-202202/usercase/2-sop-gc.md)
      - [最佳实践 | tidb-lightning 使用 tidb-backend 模式导入优化](./tidb-202202/usercase/3-tidb-lightning.md)
      - [原理解读 | Region Cache 缓存和清理逻辑解释](./tidb-202202/usercase/4-region-cache.md)

   - [TiDB 产品资讯](./tidb-202202/product-news/update.md)
      - [发版计划](./tidb-202202/product-news/1-roadmap.md)
      - [严重 Bug 及兼容性问题](./tidb-202202/product-news/2-bug.md)

   - [TiDB 社区动态](./tidb-202202/community-news/community-news.md)
      - [2 月 MOA/MVA](./tidb-202202/community-news/1-mva.md)
      - [TiDB 能力认证](./tidb-202202/community-news/2-tidb-certification.md)


- [2022 年 3 月刊](./tidb-202203/prefix-mar.md)
   - [产品更新](./tidb-202203/product-news/update.md)
      - [TiDB 6.0 发版：向企业级云数据库迈进](./tidb-202203/product-news/tidb-6.0.md)
      - [TiFlash 开源了](./tidb-202203/product-news/tiflash.md)
      - [TiFlash 快速上手资源汇总](./tidb-202203/product-news/tiflash-resource.md)

   - [产品 Bug 及兼容性问题](./tidb-202203/product-news/bug.md)
      - [TiFlash 在开启了 TLS 的情况下会随机 crash](./tidb-202203/product-news/tiflash-crash.md)
      - [TiFlash 从低版本升级到 v5.4.0 后，元数据丢失，无法响应查询请求](./tidb-202203/product-news/tiflash-v5_4_0.md)
      - [由于 Placement Rule 设置不合理导致 TiKV 节点不均衡](./tidb-202203/product-news/tikv-nodes.md)

   - [开发适配](./tidb-202203/product-news/development.md)
      - [Facebook 开源 Golang 实体框架 Ent 现已支持 TiDB](./tidb-202203/product-news/ent-tidb.md)
      - [Flink CDC 2.2 正式发布，新增 TiDB 数据源，新增 TiDB CDC 连接器](./tidb-202203/product-news/flink-tidb.md)

   - [用户实践](./tidb-202203/usercase/usercase.md)
      - [黄东旭： 关于基础软件产品价值的思考](./tidb-202203/usercase/1-infra-value.md)
      - [国产化浪潮下 TiDB 解决的痛点问题](./tidb-202203/usercase/2-inspur-tidb.md)
      - [TiDB集群恢复之TiKV集群不可用](./tidb-202203/usercase/3-tidb-cluster.md)
      - [PointGet 的一生](./tidb-202203/usercase/4-PointGet.md)
      - [TiDB 源码系列之沉浸式编译 TiDB](./tidb-202203/usercase/5-TiDB-code.md)
      - [分布式数据库TiDB在携程的实践](./tidb-202203/usercase/6-ctrip-tidb.md)
      - [网易这么牛的迁移方案你学会了吗？](./tidb-202203/usercase/7-netease-tidb.md)
      - [TiDB Server 的 OOM 问题优化探索](./tidb-202203/usercase/8-tidb-server-oom.md)
      - [混沌工程在建信金科的应用实践](./tidb-202203/usercase/9-ccbfintech-chaos.md)
      - [数据库调优之硬件](./tidb-202203/usercase/10-tuning-hardware.md)
      - [TiDB的HATP对我们来说意味着什么？](./tidb-202203/usercase/11-TiDB-HATP.md)
      - [TiEM 初体验](./tidb-202203/usercase/12-TiEM-test.md)

   - [社区动态](./tidb-202203/community-news/community-news.md)
      - [近期活动预告](./tidb-202203/community-news/event-summary.md)
      - [本月精彩活动回顾](./tidb-202203/community-news/upcoming-event.md)
      - [3 月社区 MVA（Most Valuable Advocate）](./tidb-202203/community-news/MVA-202203.md)
      - [Contributor 动态](./tidb-202203/community-news/contributors.md)

   - [TiDB 能力认证](./tidb-202203/tidb-certification/tidb-certification.md)
      - [认证介绍 & 考试安排](./tidb-202203/tidb-certification/pcta-pctp.md)
      - [TiDB 课程推荐](./tidb-202203/tidb-certification/tidb-course.md)
