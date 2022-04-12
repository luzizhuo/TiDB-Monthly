# 黄东旭： 关于基础软件产品价值的思考

**作者：PingCAP CTO 黄东旭**



前几年偶尔会写一些关于 TiDB 产品功能解读的文章，TiDB 5.0 发了那么长时间了，也应该写一写了。我其实在多个场合里表达过我对于 5.0 的重视，这个版本可能是对于 TiDB 来说的 MySQL 5.x，熟悉 MySQL 生态的朋友肯定知道我在说什么，MySQL 5.x，尤其是 5.5~5.7 这几个重要的版本基本上为 MySQL 的快速扩张奠定了坚实的基础，也培养了一大批 MySQL 的用户和人才。

对我而言，TiDB 是一个绝佳的样本，在此之前，中国本土很少有这样从零到一做出来的开源基础软件产品，多数工程师和产品经理都是这些软件的“使用者”，更多的是构建业务系统，而 TiDB 让我们第一次得以“设计者”的视角参与其中：每一个功能特性的设置背后的思考，对基础软件产品的价值呈现，体验还是很不一样的，借着这篇文章写点感受，另外这个文章是春节前我在 PingCAP 内部给 Presales 和 PM 培训上的分享整理而成，不一定正确，仅供参考。

**1）我们做的事情，对于用户意味着什么？**

要讲好基础软件的产品价值，首先要克服的第一个关卡：**学会换位思考。**其实 TiDB 每个版本都带着数十个的特性和修复，但是多数时候我们的 Release note 只是忠实的反映了 “我们做了什么”：

TiDB 4.0 GA 的 Release Note：

![f293a8cdf55c5c1a59a36eeeb0aaa1d7.png](https://tidb-blog.oss-cn-beijing.aliyuncs.com/media/f293a8cdf55c5c1a59a36eeeb0aaa1d7-1646634099856.png)



各位这里请不要理解错我的意思，这种类型的记录是很有必要存在的，但是仅有这个是远远不够的。例如在 TiDB 5.0 ~5.5 的版本里面，我们引入了 n 多的新特性：聚簇索引，异步提交事务模型，优化了 SQL 优化器，支持了 CTE，引入了锁视图和持续性能诊断工具，改进了热点调度器，降低了获取 TSO 的延迟，引入 Placement Rules SQL…这些名字在 TiDB 的开发者看来是没问题的，但是请注意，更重要的问题是：**这些对于用户（客户）意味着什么？**

要回答这个问题的思路有两种，我分别讲讲：

1. 通过一个假想的目标场景，然后通过产品去满足这个场景来展现价值。
2. 解决现有的方案（包括自己的老版本）那些最恼人的问题来展现价值。

对于第一种思路，通常适用于比较新的特性，尤其是一些过去从来没有的新鲜东西。用一个比较好理解的例子：假如大家都在开马车的时候，你发明了一个汽车，这时候如果你以汽车解决了马儿要吃草的问题作为价值点显然是比较荒谬的，更合理的是描绘高速通勤的场景带来的便利性作为卖点。这种思路有两个关键点：

1. 首先这个场景最初是产品经理假想的（当然肯定也会做过很多访谈和田野调查），所以如何确保这个场景是 “高价值” 且“具有普适性”的？对于一个成功的基础软件，这点尤其重要，通常在项目早期能抓到一个这样的点，就相当于成功了一半，当然这个对产品经理的要求是非常高的，通常需要有很强的 vision 和推动力，这就是为什么很多产品型公司的 CEO 都是早期的大号产品经理，因为在项目的早期 CEO 需要同时拥有这两样。当然更强的犹如乔布斯这种现实扭曲场，无中生有造出 iPod / iPhone 改变了整个世界，这是何等的魄力和远见（我相信 Jobs 在构思 iPhone 的时候应该能想象到今天的世界）。这个没啥办法，基本就是靠人。
2. 你产品的价值是否在这个场景里有最直接的体现。最好的直接通常是直指人心的，是人直接能体会到的“感受”。对于开发者产品来说，我通常会选择的锚点是 “用户体验”，因为好的体验是不言自明的，汽车和马车对比在通勤舒适度和效率的时候，是完胜的；就像 TiDB 和 MySQL 分库分表的方案比弹性扩展能力时候也是一样，体验上也是完胜的。对于这一点倒是有很多方法去参考，有兴趣的可以参考我那篇关于用户体验的文章。

第一种思路本质上来说是 Storytelling，这种方式的好处在于：

1. 非常好验证，当你把故事想明白了，那自然典型的用户旅程就出来了，**这时候你把自己作为一个假想的用户完整的体验一遍即是验证，**这也是我通常使用的检验我们自家产品经理工作的方式。
2. 用户很好接受，道理很简单：人人都喜欢听故事，人人都喜欢看 Demo，人人都喜欢抄作业。

对于第二种思路，通常适用于一些改进型的特性，其中的关键点在于：待解决的问题到底多痛？没有完美的软件，在重度用户那边一定会有各种各样的问题，而且这类问题通常这个功能的开发者是难以体会到的，这时候要做的也很简单，就是弯下腰去了解，去使用，去感受。我经常会去和我们的客户交付团队的一线同学聊天，在做这次分享之前也不例外，大致的对话如下：

> **我：关于我们的 SQL 优化器，你觉得日常工作中，让你最头疼的问题是啥？**
>
> Ta：执行计划突变。
>
> **我：对了，那是 hint 不太够用吗？而且 3.0 就引入了 SQL Binding？这些帮上忙了吗？**
>
> Ta：对于一些疑难杂症来说你很难通过 hint 来指定的特定的执行计划（然后附上了一个真实的业务场景中的例子，一条百行的 SQL，确实无从下手），另外 SQL Binding 问题在于，我绑定了 SQL 执行计划以后，之后如果有更好计划，还需要重新来？

> **我：我们 4.0 不如引入了 SQL Plan Management 吗？里面的自动演进功能不正好是解决这个问题的吗？**
>
> Ta：没错，但是我们生产环境都不敢开，对于极端重要的 OLTP 场景，不能容忍执行计划自动变更带来的抖动风险。
>
> **我：我们的产品做什么事情，能让你觉得日子好过一点？**
>
> Ta：1. 对于复杂的 SQL 能够选择目标执行计划，让我选择 binding 就好，而不是通过 Hint 构造；2. SPM 发现更好的执行计划，只需要及时通知我，我来做变更，而不是自动决策变更。



上面最后一句的两个反馈，我听到以后觉得很有启发，其实这两个需求都是很中肯，而且开发的代价并不大，但是确实节约了很多的时间和 DBA 的心智负担。

类似的例子还有很多，但是重点是：**找到产品的重度使用者，深入挖掘他最头疼的问题，有时候会有意想不到的收获（例如去 OnCall 的现场观察大家的操作）。**而且这类问题的解决，通常也会伴随着很好的体感，TiDB 在最近几个版本中的一些关于可观测性的改进，基本都是通过类似的观察得来。

但是第二种思路的价值展现，一定要找到合适的听众，例如：通常我们为应用开发者（数据库的使用者）解决的问题和数据库运维者（DBA）是不一样的。面对错误的对象，结果有可能会是鸡同鸭讲。



**2）当用户在说：“我要这个”的时候，Ta 其实在说什么？**

在中国基础软件的产品经理和解决方案工程师难找，我觉得是有历史原因的，就像上面我提到，过去很长时间，我们通常是站在一个“使用者”的视角去看待软件，这意味着从问题到解决方案通常是明显的，例如，假设我需要做一个高性能，支持亚毫秒低延迟读写的 User Profile 系统，数据量不大，可以容忍数据丢失，那我就用 Redis 好了！但是作为 Redis 的产品经理来说，ta 很难为了 User Profile 这个很特化的场景去设计 Redis 的功能。

![cf0e8e2b0478a52c40909f75919a8a60.png](https://tidb-blog.oss-cn-beijing.aliyuncs.com/media/cf0e8e2b0478a52c40909f75919a8a60-1646634113034.png)

优秀的基础软件产品经理通常会选择通用的技能点，用尽可能小的功能集合来包含更大的可能性（这样的灵活性是被鼓励的，例如：UNIX），所以这就对于基础软件厂商的售前和解决方案工程师提出了更高的要求：**很多业务需要的“特性”是需要多个“技术点”组合出来的，或者通过引导到正确的问题从而提供更好的解决方案。**下面我会通过几个例子来说明这个观点：

第一个例子，我们的经常被用户问到：TiDB 有没有多租户功能？这个问题的我的回复并不是简单的“有”或者“没有”，而是会去挖掘用户真正想要解决的问题是什么？潜台词是什么？在多租户的例子中大概逃不出下面几种情况：

1. 潜台词 1：“每个业务都部署一套 TiDB，太贵了”，价值点：**节约成本**
2. 潜台词 2：“我确实有好多套业务使用 TiDB, 对我来说机器成本不是问题，但是配置管理太麻烦，还要挨个升级，监控什么的还不能复用”，价值点：**降低运维复杂度**
3. 潜台词 3：“我有些场景特别重要，有些场景没那么重要，需要区别对待，对于不重要的我要共享，但是对于重要的要能隔离”，价值点：**资源隔离 + 统一管控**
4. 潜台词 4：“我有监管要求，例如不同租户的加密和审计”，价值点：**合规**

搞清楚情况后，对于这几种不同的情况，我就拿其中一个作为例子：节约成本，展开说说。下一步就是思考我们手上有什么菜了。

对于 TiDB 5.x 来说，大致有下面几个技术点和上面这个特性相关：

1. Placement Rule in SQL（灵活的决定数据放置的功能）
2. TiDB Operator on K8s
3. XX（PingCAP 的一个新的产品，暂时还没发布，请期待，大致是一个多集群可视化管控的平台）
4. TiDB Managed Cloud Service

对于节约成本的诉求，通常的原因是冷热数据比例比较悬殊，我们观察到多数大集群都符合 2/8 原则，也就是 20% 的数据承载 80% 的流量，而且尤其是对于金融类型业务，很多时候数据是永远不能删除的，这就意味着用户也需要为冷数据支付存储成本，这种情况按照统一的硬件标准去部署这其实是不划算的，所以站在用户的角度，是很合理的诉求。

下一步需要思考的是：**世界上没有新鲜事，用户现在是通过什么办法解决这样的问题呢？**

类似冷热分离这样的场景，我见过比较多的方案是冷数据用 HBase 或者其它比较低成本数据库方案（例如 MySQL 分库分表跑在机械磁盘上），热数据仍然放在 OLTP 数据库里，然后定期按照时间索引（或者分区）手动导入到冷数据集群中。这样对于应用层来说，就要知道的哪些数据去哪里查询，相当于需要对接两个数据源，而且这样的架构通常很难应对突发的冷数据读写热点（尤其是 ToC 端业务，偶尔会有一些“挖坟”的突发流量）。

然后下一个问题是：**我们的产品解决这个问题能给用户带来哪些不一样？**如果还是需要用户手动做数据搬迁，或者搭建两个配置不同的 TiDB 集群，那其实没什么大的区别，在这个场景里面，如果 TiDB 能够支持异构集群，并且自动能将冷热数据固化在特定配置的机器上，同时支持冷数据到热数据自动交换，对用户来说体验是最好的：一个 DB 意味着业务的改动和维护成本最低。在 TiDB 5.4 里面发布了一个新的功能，叫做 Placement Rules in SQL, 这个功能可以让用户使用 SQL 声明式的决定数据的分布策略，自然可以指定冷热数据的分布策略。更进一步， 对于多租户要求的更复杂数据分布方式，例如不同租户的数据放置在不同的物理机上，但是又能通过一个 TiDB 集群统一管控，通过 Placement Rules in SQL 这个功能也能实现。



**3）Meta Feature：解决方案架构师的宝藏**

说到这里，我想进一步展开一个概念，有一些功能和其它功能不一样，这类功能可以作为构建其它功能的基础，组合出新的特性。这类功能我称之为：Meta Feature，上面提到的 Placement Rule 就是一个很典型的 Meta Feature, 例如：Placment Rule + Follower Read 可以组合成接近传统意义上的数据库一写多读（但是更灵活，更加细粒度，特别适合临时性的捞数或者做临时的查询，保证数据新鲜的情况下，不影响在线业务），Placement Rule + 用户自定义的权限系统 = 支持物理隔离多租户；Placement Rule + Local Transaction + 跨中心部属 = 异地多活（WIP）；Placement Rule 还可以将精心设施数据的放置策略，让 TiDB 避免分布式事务（模拟分库分表），提升 OLTP 性能。

![be7160edd4ccf0817b6e1b5a68619ce4.png](https://tidb-blog.oss-cn-beijing.aliyuncs.com/media/be7160edd4ccf0817b6e1b5a68619ce4-1646634122120.png)

Meta Feature 通常不太会直接暴露给终端的用户，因为灵活性太强，用户会有一定的学习成本和上手门槛（除非经过精心的 UX 设计），但是这类能力对于架构师 / 解决方案提供商 / 生态合作伙伴尤其重要，因为 Meta Feature 越多，一个系统的“可玩性”越高，造出来的差异化方案也越多。但是通常我们会犯一个错误：灵活性是否等于产品价值？我认为不是的，虽然工程师（尤其是 Geek）对这类开放能力有天生的好感，但是对于终端用户到底能否说好这样的故事，我是存疑的，看看 Windows 和 UNIX 的终端用户的市场占有率就知道了。在这个例子上最近我听到了个绝佳的例子，和大家分享：**你并不能对一个美式的爱好者说拿铁更好，因为你可以灵活的控制含奶量，奶量降低到 0 就包含了美式。**

我们再看一个场景，关于批处理。熟悉 TiDB 历史的朋友肯定知道我们最早这个项目的初心其实是从 MySQL Sharding 的替换开始的，后来慢慢的很多用户发现：反正我的数据都已经在 TiDB 里了，为什么不直接在上面做计算？或者原来一些使用 SQL 做的复杂的数据变换工作遇到了单机计算能力瓶颈，而且因为一些业务要求，这些计算还需要保持强一致性甚至 ACID 事务支持，一个典型的场景就是银行的清结算业务。

本来年轻的我还不太理解，这类批处理业务直接 Hadoop 跑就好了，后来了解清楚情况以后才发现还是年轻了，对于银行来说，很多传统的清结算业务是直接跑在核心的数据库上的，而且业务也不简单，一个 Job 上百行的 SQL 家常便饭，很可能开发这个 Job 的开发商已经不见了，谁也不敢轻易改写成 MR Job，另外对于批量后结果，可能还要回填到数据库中，而且整个过程需要在短短几个小时内完成，完不成就是生产事故。原本如果数据量没那么大，跑在 Oracle，DB2 小型机上也没啥问题，但是最近这几年随着移动支付和电子商务的兴起，数据量越来越大，增长也越来越快，Scale-Up 一定迟早成为瓶颈。TiDB 在其中正好切中两个很高的价值点：

1. SQL 兼容的能力（尤其在 5.0 支持 CTE 后和 5.3 引入的临时表功能，复杂 SQL 的兼容性和性能得到很好提升），也支持金融级的一致性事务能力。
2. Scale-out 横向的计算扩展能力（尤其在 5.0 支持 TiFlash MPP 模式后，解锁了在列式存储上进行分布式计算的能力），理论上只要有足够多的机器，吞吐能够扩展上去。

对于银行的批量业务来说，令人头疼架构改造问题变成了简单的买机器的问题，你说香不香？但是在 TiDB 早期设计的解决方案中，有几个痛点：

1. 大批量数据导入
2. 分布式计算

对于第一个问题，通常一个典型的 TiDB 做批量任务的流程是：下档（每日的交易记录通过文件的形式发布）-> **将这些记录批量写入到 TiDB 中** -> **计算（通过 SQL）** -> 计算结果回填到 TiDB 的表中。档案记录可能是一大堆文本文件（例如 CSV）格式，最简单的写入方式肯定就是直接一条条记录用 SQL Insert 的方式写入，这个方式处理点小数据量问题不大，但是数据量大的话，其实是比较不划算的，毕竟大多数导入都是离线导入，虽然 TiDB 提供大事务（单个事务最大 10G），但是站在用户的角度有几个问题：

1. 批量写入通常是离线的，这种场景用户的核心诉求是：快！在这种场景下，完整的走完分布式事务的流程是没必要的。
2. 虽然有 10G 的边界，对于用户来说也很难切割得精确。
3. 大事务的写入过程中意味着需要更大的内存缓存，这点常常被忽略。

一个更好方式是支持物理导入，直接分布式的生成底层存储引擎的数据文件，分发给存储节点，直接插入物理文件，也就是 TiDB 的 Lightning 做的事情。在最近的一个真实用户的场景观察到，Lightning 使用 3 台机器，大概在 72h 内完成了 ~30T 的原始数据的转码和导入工作，大概导入吞吐能做到 380GB/h。所以在批量的场景中，能使用 Lightning 物理导入模式的话，通常是一个更快且更稳定的解。

另外的一个痛点，计算瓶颈（听起来还挺不合理的，哈哈哈），在早期 TiDB 还不支持 MPP 的时代，TiDB 只支持 1 层的算子下推，也就是 Coprocessor 分布在 TiKV 中的计算的结果只能汇总在一台 TiDB 计算节点上进行聚合，如果中间结果过大，超过了这个 TiDB 节点的内存，就会 OOM，这也就是为什么过去 TiDB 需要引入 Spark 来进行更复杂的分布式计算的原因（尤其是大表和大表的 Join），所以在过去对于复杂的批量业务还是需要引入一批 Spark 的机器通过 TiSpark 对 TiDB 的计算能力进行补充。但是在 TiDB 5.0 后引入了 TiFlash 的 MPP 模式，可以通过多个 TiDB 计算节点进行计算结果聚合，于是计算能力并不再是瓶颈了，这意味着，很有可能在一些 TiDB 的批量计算场景中，5.0 能够节省一批 Spark 的机器，意味着更简单的技术栈和更高的性能。

更进一步，引入 Spark 还有一个原因，就是在计算结果回填的阶段，由于 TiDB 的事务大小限制，以及提升并发写入的效率，我们会使用 Spark 来对 TiDB 进行分布式的数据插入。这个过程理论上也是可以通过 Lightning 改进的，TiFlash MPP 可以将结果数据输出成 CSV，Lightning 是支持 CSV 格式的数据导入的。

所以原来的链路理论上有可能变成：Lightning 导入数据到 TiDB -> TiDB 使用 TiFlash MPP 进行计算结果输出成 CSV -> 再次通过的 Lightning 将 CSV 结果写入到 TiDB 中；有可能比使用 TiSpark + 大事务的方案更快更省资源，也更稳定。

在这个方案上我们可以再延伸一下仔细想想，上面的方案优化其实是利用了 Lightning 的大批量数据写入能力，理论上有“大写入压力”的数据导入场景，都可以通过这个思路改进。我这里分享一个 TiDB 的用户真实反馈：这个客户业务系统上到 TiDB 后，会有定期大表导入的场景，他们希望先将大空表通过 Placement Rule 指定到特定空闲主机，然后通过 Lightning 快速导入数据，不需要考虑限流等措施也可以降低对整体集群的影响，实现快速导入；相反的，如果 TiDB 没有这个调度能力，客户只能通过限流的方式保持集群稳定，但是导入速度会很慢。这个例子是通过 Placement Rule + Lightning 实现了在线的批量写入，也是很好的呼应了一下前面关于 Meta Feature 的描述。

本来在线下的分享中还有关于“分库分表” vs TiDB 的例子，因为篇幅关系就不展开了，感兴趣的可以按照上面的思路去思考。



**4）更隐式，但更大更长期的价值：可观测性和 Troubleshooting 能力**

最后一部分，大家也能看到，最近其实我一直在努力的传达这个 Message，对于一个基础软件产品来说，一个重要的长期竞争力和产品价值来自于可观测性和 Troubleshooting 能力。这个世界没有完美的软件，而且对于有经验的开发者来说，快速的发现和定位问题的能力是必备的，对于基础软件的商业化来说，服务支持效率和 Self-serving 也是规模化的基础，这一点在云的环境下也同样重要。我这里说一些我们最近做的一些新的事情，以及未来面临的挑战。

TiDB Clinic (tiup diag) 

为什么要做这个事情？过去我们在做故障诊断的时候，是一个痛苦的过程，除了我在之前的关于可观测性的文章中提到的老司机的经验只在老司机脑子里的问题外，我观察到其实消耗时间的大头来自于收集信息，尤其是部署在用户自己的环境中，用户对于系统诊断并不熟悉，求助我们的服务支持的时候，经常的对话是：

> **服务支持：请运行这个命令 xxx，然后告诉我结果**
>
> 客户：（2 小时后才给了结果）
>
> **服务支持：不好意思，麻烦在你们的监控界面上看某个指标的图表**
>
> 
>
> 客户：截图给你了
>
> **服务支持：不好意思的，时间段选错了。。。然后调整一下 grafana 的规则，再来一遍**
>
> 
>
> 客户：！@##￥#￥%
>
> **服务支持（隔了几天换了个人值班）：请运行这个命令 xxx，然后告诉我结果**
>
> 客户：之前不是给过了吗？



这样一来一回异步又低效的问题诊断是很大的痛苦的来源，以及 oncall 没办法 scale 的核心原因之一。用户的痛点是：

1. 你就不能一次性要完所有的信息吗？我并不知道给你哪些”
2. “信息太大太多太杂，我怎么给你？”
3. “我的 dashboard 在内网里，你看不到，我也只能截图”
4. “我不能暴露业务信息，但是可以提交诊断信息”

但是反过来，TiDB 的服务支持人员的痛点是：

1. “原来猜测的方向不太对，需要另一些 metric 来验证”
2. “无法完整重现故障现场的 metrics 和系统状态，我希望自由的操作 Grafana”
3. “不同的服务支持人员对于同一个用户的上下文共享”

于是就有了 Clinic 这个产品了，在用户同意的前提下：

1. 通过 tiup 一键自动收集和系统诊断相关的各种指标
2. 通过不断学习的规则引擎，自动化诊断一些常见错误
3. 针对不同租户的诊断信息存储和回放平台（类似 SaaS）

如果熟悉 AskTUG （TiDB 用户论坛）的朋友，可能会看到类似这样的链接：https://clinic.pingcap.net/xxx（例如这个 case：https://asktug.com/t/topic/573261/13）

对于用户来说，只需要在集群内执行一个简单的命令，就会生成上面这样的一个链接，把重要的诊断信息与 PingCAP 的专业服务支持人员共享，我们在后台可以看到：

![aa8800c6ceebe3a17a596e00f817d0a3.png](https://tidb-blog.oss-cn-beijing.aliyuncs.com/media/aa8800c6ceebe3a17a596e00f817d0a3-1646634134519.png)

![6e5f423adce95450b79429e16ea43981.png](https://tidb-blog.oss-cn-beijing.aliyuncs.com/media/6e5f423adce95450b79429e16ea43981-1646634143058.png)

其实 TiDB Clinic 也是对于基础软件的维护性的一个新尝试：诊断能力的 SaaS 化，通过一个在云端不断强化的规则引擎，将故障的诊断和修复建议和本地的运维部署结耦。这样的能力会变成用户选择 TiDB 的一个新的价值点，也是 TiDB 很强的生态护城河。

TiDB Dashboard 中 Profiling 

我心目中对于一个基础软件产品是不是好，我有一个特别的标准：**自带 Profiler 的，基本上都是良心产品，能够把 Profile 体验做 UX 优化的，**更是良心中的良心。例如 Golang 的 pprof，用过都说香。其实这个点说起来也不难做，但是关键时刻能救命，而且通常出事的时候也没法 Profile 了，这个时候如果系统告诉你，自己在故障的时候保存了一份当时的 profile 记录，这种雪中送炭似的帮助的体验是很好的。

![9aa6bae10d8f93d63298b86e74836391.png](https://tidb-blog.oss-cn-beijing.aliyuncs.com/media/9aa6bae10d8f93d63298b86e74836391-1646634149266.png)

![ad191a8601aa779a9555352a78e6122b.png](https://tidb-blog.oss-cn-beijing.aliyuncs.com/media/ad191a8601aa779a9555352a78e6122b-1646634155561.png)

其实这个功能来自于几个我们实际处理过的 oncall case，都是一些通过 metric 没法覆盖到的问题，有一大类故障，是遇到硬件瓶颈了，大概逃不过 CPU 和磁盘，磁盘瓶颈相对好查，大致看有没有大的 IO（Update / Delete / Insert）或者  RocksDB 本身的 Compaction 就好，但是 CPU 瓶颈的查找方式就模糊许多，Profiler 几乎是唯一的方式：

1. CPU 的关键路径上的 Call Stack 是什么样子
2. 这些关键路径上的函数调用暗示了什么？第二个问题通常是查问题的关键，会给出一个优化的方向，例如我们发现 SQL Parse/ 优化的 CPU 消耗特别大，这就暗示了应该使用 Plan Cache 这样的机制能够提升 CPU 的利用率。

目前 TiDB 在 5.x 中提供了两种 Profile 方式：手动 Profile 和自动持续 Profile，两种应用场景不同，手动的通常用于针对性的性能优化；自动持续 Profile 通常用于系统出现问题后的回溯。



**5）面临的挑战**

快结尾了，说点挑战。PingCAP 是在 2015 年成立的，到现在已经马上就要 7 岁了，在这 7 年里，正好经历了一些很重要的行业变革：

1. 数据库技术从分布式系统过渡到云原生；虽然很多人可能觉得这两个词并不是一个层面上的概念，因为云原生也是分布式系统实现的呀？但是我觉得云原生是一种设计系统的思考方式的根本改变，这点我在我其他很多文章里提过，就不赘述了。
2. 开源的数据库软件公司找到了可规模化商业化的模式：在云上的 Managed Service。
3. 全球的基础软件领域正在经历从“能用”变成“好用”的阶段

这几点分别代表两个方向上的认知改变：

1. 在技术上，需要完成从依赖计算机的操作系统和硬件变成依赖云服务，这一点对于技术的挑战是巨大的，例如：使用 EBS 的话，是否 Data Replication 还是必须的？使用 Serverless 的话，是否能够打破有限的计算资源的限制？如果这个问题再叠加上已有的系统可能有大量的现有用户会变得更加复杂，当然，云原生技术并不等于公有云 Only，但如何设计出一条路径，慢慢的过渡到以云原生技术为基础的新架构上？这会是对于研发和产品团队一个巨大的挑战。
2. 第二个改变会是更大的挑战，因为商业模式在转变，在传统的开源数据库公司，主流的商业模式是以服务支持为主的人力生意，高级一点是类似 Oracle 这样的卖保险的生意，但是这些商业模式都没有办法很好的回答两个问题：
3. a. 商业版和开源版的价值差异
4. b. 如何规模化，已经靠人力是无法规模化的

而 SaaS 的模式则可以很好回答这两个问题，而且基础设施类的软件和 SaaS 的模式融合后会有更大的放大效应 ，我在《大教堂终将倒下，但集市永存》一文提及过，但是真正的挑战在于：**一个面向传统软件售卖 + 服务支持导向的软件公司的组织如何调整为一个面向运营的线上服务公司？**以研发体系为视角，举几个小例子：1. 版本发布，对于传统软件公司来说，一年发布 1～2 个版本就不错了，但是线上服务有可能一个礼拜就升级一次，不要小看这个发版节奏的差异，这个差异决定了整个研发和质量保障体系模式的差异。2. 如果在云上提供服务，那么就需要配套的运营支持系统（计费，审计，故障诊断等）以及相应的 SRE 团队，这些系统可能并不在传统的软件研发体系里面，另外对于用户体验和开发者体验的关注变得尤其重要。

当然挑战也不止这些，也都没有标准答案，不过我还是对未来充满信心的，毕竟这些趋势本质上都是在加速技术到社会价值和商业价值的转化以及降低门槛，都是好的且务实的转变，这对于 PingCAP 这样的公司来说当然是利好，前方是星辰大海，一切都是事在人为，大有可为。本来就想写一篇小文，没想到写着写着就扯得有点长了，就到这里吧。