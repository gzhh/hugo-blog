---
title: "【阅读笔记】Understanding Distributed Systems"
date: 2022-08-15T00:30:07+08:00
draft: false
categories: [Read, DistributedSystems]
tags: [DistributedSystems]
---

{{< toc >}}

## 1. Introduction
设计、搭建、操作分布式系统的挑战主要可以从以下几个方面来展开
- Communication
- Coordination
- Scalability
- Resiliency
- Maintainability



## Part I: Communication
### 2. Reliable links
使用 TCP 传输层协议，在 IP 的基础上为两个进程提供可靠的通信通道。

**可靠性**
使用带序列号的段来传递数据，并引入重发和 checksum 机制确保数据传输不会被破坏。

**连接生命周期**
TCP 三次握手的引入。

**流量控制**
流量控制是TCP实现的一种回退机制，以防止发送方发送数据超过接收方处理能力的情况。

**拥塞控制**
发送方维护一个拥塞窗口，它代表着可以在没有接收方确认的情况下发送的未完成段的总数。


### 3. Secure links
在应用层和传输层之间引入 TLS 层来确保数据通信的安全性。

- 加密：对称加密
- 认证：certificate
- 完整性：TLS HMAC
- TLS 握手


### 4. Discovery
使用 DNS 来解析服务的 IP 地址。

经典问题：在浏览器中输入一个网址背后发生一系列工作的过程。


### 5. APIs
使用 HTTP API（或 RPC）等请求响应这样的同步方式来实现客户端和服务端的通信。

使用 HTTP API 需要关注的地方
- 资源：使用 URL 定义一个资源
- 请求方法
- 响应状态码
- OpenAPI：使用 Swagger 这样的工具来统一管理 RESTful HTTP APIs 的 IDLs
- Evolution: 使用 REST 来规范化 API 的设计
- 幂等性：使用唯一标志符（例如 UUID）来确保客户端不管多次相同请求后都有一致的结果



## Part II: Coordination
### 8. Time

参考：
[时钟介绍](https://www.raychase.net/5768)


### 9. Leader election
**Raft**

参考：
- [The Raft Consensus Algorithm](https://raft.github.io/)
- [Raft algorithm](https://en.wikipedia.org/wiki/Raft_(algorithm))


### 10. Replication
数据复制技术是构建分布式系统的基础环节，主要有两个原因：
- 提高可用性：如果访问数据失败，客户端可以切换到访问备份数据
- 提高伸缩性和性能：有越多的副本，就有更多的客户端能并发访问

使用 Raft’s replication protocol 来确保数据一致性（Paxos 也可以，但是 Raft 更容易理解）

**状态机复制 State machine replication**

参考：
- [State machine replication](https://en.wikipedia.org/wiki/State_machine_replication)

**分布式共识 Consensus**

参考：
- [Consensus](https://en.wikipedia.org/wiki/Consensus_(computer_science))

**一致性模型 Consistency models**
- 强一致性 Strong consistency
  - 任何对一个内存位置X的读操作，将返回最近一次对该内存位置的写操作所写入的值
- 顺序一致性 Sequential consistency 
  - 与强一致性对比，顺序一致性缺少实时保证
  - 队列的生产者/消费者系统就是这个模型的一个例子
- 最终一致性 Eventual consistency
  - 对于已改变写的数据的读取，最终都能取得已更新的数据，但不完全保证能立即取得已更新的数据
- CAP 理论
  - [CAP theorem](https://en.wikipedia.org/wiki/CAP_theorem)
  - 总结为：strong consistency, availability and partition tolerance: pick two out of three. In reality, the choice really is only between strong consistency and availability, as network faults are a given and can’t be avoided.
  - CAP 理论的扩展：PACELC 理论

**链复制 Chain replication**
- 介绍：Chain replication is a widely used replication protocol that uses a very different topology from leader-based replication protocols like Raft. In chain replication, processes are arranged in a chain. The leftmost process is referred to as the chain’s head, while the rightmost one is the chain’s tail.
- 目标：To achieve high throughput and availability without sacrificing strong consistency.


### 11. Coordination avoidance
There is a tradeoff between consistency and availability/performance. In other words, to build scalable and available systems, coordination needs to be minimized.


### 12 . Transactions
**ACID**

**隔离性 Isolation**
- 并发控制 Concurrency control
  - two-phase locking (2PL)
  - Optimistic concurrency control (OCC)
  - Multi-version concurrency control (MVCC)

**原子性 Atomicity**
- Two-phase commit(2PC)

**NewSQL**


### 13. Asynchronous transactions
2PC 是一种同步阻塞协议，会有很多问题，为了解决这些问题，所以引入了异步事务。

**Outbox pattern**
- 具体实现可以参考消息中间件，比如 Kafka

**Saga**

参考：[Pattern: Saga](https://microservices.io/patterns/data/saga.html)

**总结**

两个关键点
- 在分布式系统中节点故障不可避免
- 节点协调代价高



## Part III: Scalability
在百万级的并发访问系统中，需要可伸缩性系统的支持。

### 14. HTTP caching


### 15. Content delivery networks


### 16. Partitioning
Partition 的复杂性
- 将请求路由到正确的节点
- 多个分区数据的聚合计算
- 多个分区数据的更新要遵循事务的原子性
- 节点访问要尽可能均衡
- 添加或删除节点时的数据迁移需要跨节点

范围分区 Range partitioning
- 因平衡系统的负载来调整节点的增加和减少就叫 rebalancing
- static partitioning 系统初始化时就分配固定的分区数，恒定不变，当节点数量变化时同时调整分区到节点的关系来保证平衡
- dynamic partitioning 系统初始时分配一个分区，数据变多时进行分区拆分，数据变少时进行分区合并

Hash 分区 Hash partitioning
- [consistent hashing](https://en.wikipedia.org/wiki/Consistent_hashing)
- 与 Range partitioning 相关，Hash partitioning 最主要的缺点是会失去数据的排序顺序


### 17. File storage
静态文件存储
- Azure 的 Blob 对象存储
- 阿里云的 OSS 对象存储


### 18. Network load balancing
负载均衡器的核心特性：
- Load balancing
- Service discovery
- Health checks

**DNS load balancing**
- 使用 DNS 来实现负载均衡

**Transport layer load balancing**
- 使用网络栈的 TCP 协议来实现负载均衡，也叫四层负载均衡，比 DNS 实现的更灵活，可以映射多个虚拟 IP。
- 缺点：由于是基于 TCP 协议按字节流传输数据，所以不能使用像 TLS 这样更高层的协议提供的特性。

**Application layer load balancing**
- 使用网络栈的 HTTP 协议来实现负载均衡，也叫七层负载均衡，是一个 HTTP 反向代理。
- 可以让多个 HTTP 请求可以共用一个 TCP 连接
- 可以在流量控制方面做的更智能：
  - 基于 HTTP Header 的限流
  - 终结 TLS 连接
  - 强制 HTTP 请求归属到相同的逻辑会话上，即路由到相同的后端服务器
- 也可以提供 service mesh 能力，像 Nginx, Envoy 等


### 19. Data storage
**Relication**
- 创建只读副本来增加读能力和可用性
- 同步和异步结合更能提高复制的效果

**Partitioning**
- 数据分区可提升数据存储的扩容能力

**NoSQL**


### 20. Caching
**Policies**
- LRU
- TTL

**Local cache**
- 本地内存缓存，例如基于内存的哈希表或者嵌套键值存储

**External cache**
- Redis
- Memcached


### 21. Microservices
当系统变得越来越庞大时，往往需要从单体应用架构迁移到微服务架构。

**注意点**
- 技术栈
- 沟通协调
- 耦合
- 资源调配：配置自动化
- 测试
- 维护：CI/CD
- 最终一致性

**API gateway**
- 暴露公网 API 的服务叫做 API gateway（一种反向代理）

- 核心职责
  - Routing：路由请求到具体的子服务
  - Composition：组合多个子服务返回的数据
  - Translation：可以转化 IPC 的机制，例如将 Restful HTTP 请求转换为内部 RPC 调用
- Cross-cutting concerns 横切关注点
 <br>软件系统，可看作由一组关注点组成。其中，直接的业务关注点，是直切关注点。而为直切关注点提供服务的，就是横切关注点。
  - 日志
  - 监控
  - 限流
  - 认证授权
- 注意点
  - API gateway 最大的缺点是可能成为开发瓶颈，因为其与内部服务是高度耦合的


### 22. Control planes and data planes
可以将 API gateway 划分为 data plane 和 control plane
- 控制平面 control plane: manages the gateway’s metadata and configuration
- 数据平面 data plane: serves external requests directed towards our internal services

总结：
- More generally, a data plane includes any functionality on the critical path that needs to run for each client request. Therefore, it must be highly available, fast, and scale with the number of requests. In contrast, a control plane is not on the critical path and has less strict scaling requirements. Its main job is to help the data plane do its work by managing metadata or configuration and coordinating complex and infrequent operations. And since it generally needs to offer a consistent view of its state to the data plane, it favors consistency over availability.
- Try to make the control plane more reliable, but more importantly, we should ensure that the data plane can withstand control plane failures.

**Scale imbalance**
- 面对数据平面开启时无法访问控制平面这种情况，可以使用第三方对象存储来解耦，避免控制平面过载，也可以保证控制平面无法使用时数据平面能正常操作。
- 当大量数据平面实例在同一时间启动时（大规模扩容或重启），尝试去从控制切片读全量的配置，可能会导致控制切片出现问题。为了防止这种情况，可以读最近配置的快照来进行初始化。

**Control theory**
- Goal is to create a controller that monitors a dynamic system, compares its state to the desired one, and applies a corrective action to drive the system closer to it while minimizing any instabilities on the way.
- The data plane is the dynamic system we would like to drive to the desired state, while the controller is the control plane responsible for monitoring the data plane, comparing it to the desired state, and executing a corrective action if needed.

### 23. Messaging
消息中间件在分布式系统中能起到很好的解耦效果，消息中间件通信风格有以下几种：
- One-way messaging: point-to-point
- Request-response messaging
- Broadcast messaging: publish subscribe

**Guarantees**
消息代理有许多不能保证，只能尽量权衡的点：
- the order of messages;
- delivery guarantees, like at-most-once or at-least-once;
- message durability guarantees;
- latency;
- messaging standards supported, like AMQP;
- support for competing consumer instances;
- broker limits, such as the maximum supported size of messages.

**Exactly-once processing**
- 因为不能保证消息只被发送一次，主要有两个原因
  - 在处理之前就删除了消息，此时系统崩溃，消息未处理，导致消息丢失
  - 在处理之后删除消息，此时系统崩溃，导致消息未被删除，系统重启后导致消息被重复处理
- 所以为了保证消息只被处理一次，要求消费者要做到幂等处理，并且只有在消息处理完成后才从 channel 中删除消息。

**Failures**
- 消费者处理消息失败时有几种办法：
  - 重试，尽可能多的重试
  - 将多次重试后无法处理的消息加入 dead letter channel，然后再从 main channel 删除
  - 追踪消费失败的原因，并解决，再将消息放回 main channel 重新消费

**Backlogs 堆积**
- 处理消息堆积的办法：
  - 加监控，及时报警，及时处理

**Fault isolation**
- 对于消息者发送的异常消息（如果可以标记出来），可以在将其发到 dead channel 之前，主动将其放到 low-priority channel，减少消耗 main channel 的资源



## Part IV: Resiliency
因为系统不可避免的可能会发生故障，那么当系统出现故障时，就需要更快的去探测、响应、修复问题。

弹性是指通过快速响应故障并在故障后完全恢复来避免或减轻不良事件影响的能力，注重弹性通常意味着强调高可用性，这样可以增加正常运行时间。

### 24. Common failure causes
一些常见造成系统故障的根本原因：
- 硬件故障
- 错误处理不当
- 配置变更
- 单点故障
- 网络故障
- 资源泄露
- 负载压力
- Cascading failures 级联故障
- 风险管理


### 25. Redundancy
冗余可以增加系统的可用性

- 关联性：
  - 降低关联性来提高系统的可用性，例如可以使用多源数据中心来降低所有数据服务同时出现故障的情况。


### 26. Fault isolation
对于高度关联的系统，我们不能使用冗余来容错。但是可以使用 partition 来进行故障隔离。

**Shuffle sharding 随机分片**
- 对于无状态服务，可以使用随机分片来进行故障隔离

**Cellular architecture 蜂窝架构**
- 一个集群中通常有多个服务，通过增加集群的数量来进行故障隔离，而不是在集群中增加服务的数量。

### 27. Downstream resiliency
为了保护服务不受下游依赖的影响，我们在以下这些方面做工作，来提高系统的弹性能力
- Timeout
- Retry
  - Exponential backoff 指数退避：即随时间推移，重试次数逐渐减少
  - Retry amplification 重试放大：调用链路上，越深层的服务，越会有更高的负载
- Circuit breaker 断路器
  - 在通过超时重试操作确认服务的下游依赖不可用时，为了避免服务无终止的重试浪费资源，一般有两种做法
  - a. 针对致命性的功能错误需要及时报错
  - b. 而对于非瞬时故障，通过断路器来保护当前服务不受影响，断路器的状态有 closed, open, half-open

### 28. Upstream resiliency
保护服务不受上游依赖的影响
- Load shedding 削减负荷：当服务的资源快被耗尽时，主动给新请求直接返回错误（based on the local state of a process，线程进程层面更多）
- Load leveling 负荷平移：使用消息中间件与上游依赖解耦，按服务自身的能力处理上游消息
- Rate-limiting 限流：使用限流保护服务不被瞬时大量的请求击垮（based on the global state of the system，网关层面更多）
  - Single-process implementation：
  - Distributed implementation：需要使用分布式事务
- Constant work 不间断运行：在配置的修改、强制报错等场景下，应该支持配置热更新、错误优雅处理的技术，保证服务不被中断



## Part V Maintainability
### 29. Testing
**Scope 范围**
- Unit test 单元测试验证小部分代码块的行为，比如一个独立的类或方法
- Integration test 集成测试验证一个服务的内部依赖是否能按预期交互
- End-to-end test 端到端测试验证系统的跨服务行为，如用户真实面对的场景
- 总结：A good trade-off is to have a large number of unit tests, a smaller fraction of integration tests, and even fewer end-to-end tests 

**Size 大小**
- 小型测试运行在单进程上，不会表现出调用阻塞或I/O阻塞，所以它很快且有决定性，并具有非常小的间歇性故障的概率
- 大型测试要求多个节点来执行，甚至有更大的不确定性和长延时
- 结论：需要尽可能的写小型测试来验证行为，使用 test double（测试替身）来代替依赖，可以使用下面这些方法来减小测试的大小
  - fake
  - stub
  - mock

**Practical considerations 实际考虑**
- 当然测试还需要考虑实际情况，比如测试支付场景时应该用 mock 代替真实的第三方支付 API

**Formal verification 形式化验证**
- 除了测试之外，我们可以通过写说明书来描述系统的行为，让我们在写代码之前就能发现细微的 bug 和架构缺陷


### 30. Continuous delivery and deployment
持续发布部署的几个阶段
- Review and build
- Pre-production
- Production
- Rollbacks


### 31. Monitoring
监控系统需要关注的点：
- Metrics：用来衡量资源使用或表现行为
- Service-level indicators 服务级指标
  - long-tail latency 长尾延迟会影响指标的准确性
- Service-level objectives 服务级目标
- Alerts
- Dashboards
- Being on call


### 32. Observability
Observability is a superset of monitoring
- Logs
- Traces


### 33. Manageability
将敏感的配置信息从代码中抽离出来，放到单独的配置中心


## Reference
[Understanding Distributed Systems](https://www.goodreads.com/book/show/56977420-understanding-distributed-systems)
