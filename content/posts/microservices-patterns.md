---
title: "【阅读笔记】微服务架构设计模式"
date: 2022-06-20T17:58:50+08:00
draft: false
categories: [系统架构]
tags: [Microservices]
---

{{< toc >}}

## 一、系统架构设计介绍

### 单体架构
定义：一个系统的所有子模块都被部署在同一台服务器上。

**单体架构的好处**
1. 应用的开发简单
2. 易于对应用程序进行大规模的更改
3. 测试相对简单直观
4. 部署简单
5. 横向扩展容易

**单体地狱**

随着不断的在单体架构的基础上扩展，每一次实现更多功能的时候，就会导致代码库膨胀。并且随着研发团队规模的不断壮大，团队的管理成本也不断提高，开发变得缓慢和痛苦。

**单体架构的弊端**
1. 过度的复杂性会吓退开发者
2. 开发速度缓慢
3. 从代码提交到实际部署的周期很长，而且容易出问题
4. 难以扩展
5. 交付可靠的单体应用是一项挑战
6. 需要长期依赖某个可能过时的技术栈

### 微服务架构
定义：将一个单体应用按照不同的功能模块进行分解，拆分成一个个小的服务，每个服务实现了一组相关的功能。

**微服务架构的好处**
1. 使大型的复杂应用程序可以持续交付和持续部署
2. 每个服务都相对较小并容易维护
3. 服务可以独立部署
4. 服务可以独立扩展
5. 微服务架构可以实现团队的自治
6. 更容易实验和采纳新的技术
7. 更好的容错性

**微服务架构的弊端**
1. 服务的拆分和定义是一项挑战
2. 分布式系统带来的各种复杂性，使开发、测试和部署变得更困难
3. 当部署跨越多个服务的功能时需要谨慎地协调更多的开发团队
4. 开发者需要思考到底应该在应用的什么阶段使用微服务架构


## 二、服务的拆分策略
### 拆分规范
**拆分规范**
1. 高内聚、低耦合
2. 服务拆分正交性
3. 服务拆分层级最多三层
4. 服务粒度适中、演进式拆分
5. 避免环形依赖、双向依赖
6. 通用化接口设计，减少定制化设计
7. 将串行调用改为并行调用，或者异步化
8. 接口应该实现幂等性
9. 接口数据定义严禁内嵌，透传
10. 避免服务间共享数据库
11. 同时应当考虑团队结构

**拆分时机**
1. 快速迭代
2. 高并发、性能要求
3. 提交代码频繁出现大量冲突
4. 小功能要积累到大版本才能上线

拆分规范参考
- [微服务拆分规范](https://zhuanlan.zhihu.com/p/333393446)
- [微服务拆分之道](https://mp.weixin.qq.com/s/mojOSgEUaHWGU3H3j7WjlQ)

### 拆分策略
**根据业务能力进行拆分**

**根据子域进行服务拆分**
- 领域驱动设计 DDD
  - [领域驱动设计在互联网业务开发中的实践](https://tech.meituan.com/2017/12/22/ddd-in-practice.html)
  - [使用 DDD 指导微服务拆分的逻辑](https://www.infoq.cn/article/mf3sggvvf69fidl26ie6)

### 拆分难点
- 网络延迟
- 同步进程间通信导致可用性降低
- 在服务之间维持数据一致性
- 获取一致的数据视图
- 上帝类阻碍了拆分（上帝类是在整个应用程序中使用的全局类）


## 三、微服务架构中的进程间通信
### RPC（同步通信机制）
**gRPC api 实现**
1. 接口定义语言 IDL
  - protocol buffer
2. 通信协议 grpc
  - 基于 http2 实现的二进制消息的协议
3. http2
  - 客户端和服务端使用 http/2 以 protocol buffer 格式交换二进制消息

**好处**
- 设计具有复杂更新操作的API非常简单
- 具有高效、紧凑的进程间通信机制，尤其是在交换大量消息时
- 支持在远程过程调用和消息传递过程中使用双向流式消息方式
- 实现了客户端和各种语言编写的服务端之间的互操作性

**弊端**
- 与基于 REST/JSON 的 API 机制相比，JavaScript 客户端使用基于 gPRC 的 API 需要做更多的工作。
- 旧式防火墙可能不支持 HTTP/2

### 同步通信机制局部故障问题
- 限流
  - 令牌桶 Token Bucket
  - 漏桶 Leaky Bucket
- 熔断
  - 介绍
    - 熔断器首先是关闭的，当主调失败到达了一定阈值后，此时熔断器打开（主调的请求不会再到被调了，压根就不会发出请求），过了一个"冷却时间"后，熔断器此时会处于一个半开的状态，会有请求到被调，如果请求成功就关闭熔断器（后端服务恢复），否则就继续打开熔断器。
    - 熔断强调的是服务之间的调用能实现自我恢复的状态。
  - 主动熔断：通过服务发现组件来获取被调的实例，如果主调的失败达到了一个阈值，服务发现组件会熔断被调的实例
  - 被动熔断：cpu/内存/io 等超过了一定的阈值，那么就会触发熔断，将抛弃进来的请求
- 降级
- 隔离
- 重试
- 超时监控
- 监控
- 报警
- 预案

### 服务发现
**概念**
- 其关键组件是服务注册表，它是包含服务实例网络位置信息的一个数据库。
- 服务实例启动和停止时，服务发现机制会更新服务注册表。当客户端调用服务时，服务发现机制会查询服务注册表以获取可用服务实例的列表，并将请求路由到其中一个服务实例。

**实现方式**
- 服务及其客户端直接与服务注册表交互
- 通过部署基础设施来处理服务发现，Docker 和 Kubernetes 等相关云原生组件

### 消息代理 MQ（异步消息通信机制）
**消息通道类型**
- 点对点通道
  一对一交互
- 发布订阅
  一对多交互

**好处**
- 松耦合
- 消息缓存
- 灵活的通信
- 明确的进程间通信

**弊端**
- 潜在的性能瓶颈
- 潜在的单点故障
- 额外的操作复杂性

**处理并发和消息顺序**

场景：
使用多个线程和服务实例来并发处理消息可以提高应用程序的吞吐量。

挑战：
确保每个消息植被处理一次，并且是按照它们发送的顺序来处理的。

解决方案：Kafka 分片
- 分片通道由两个或多个分片组成，每个分片的行为类似于一个通道。
- 发送方在消息头部指定分片键，通常是任意字符串或字节序列。消息代理使用分片键将消息分片给特定的分片。例如，可以通过计算分片键的散列来选择分片。
- 消息代理将接收方的多个实例组合在一起，并将它们视为相同的逻辑接收方。例如 Kafka 的使用术语消费者组。消息代理将每个分片分配给单个接收器。它在接收方启动和关闭时重新分配分片。

**处理重复消息**

解决方案
- 编写幂等消息处理程序
- 跟踪消息并丢弃重复项
  - 消息接收方使用 message id 跟踪它已经处理的消息并丢弃任何重复项
    - 依赖 rdms 的特性
    - 利用 nosql


## 四、管理事务
### Saga
[参考](https://microservices.io/patterns/data/saga.html)

## 五、微服务架构中的业务逻辑设计
### 业务逻辑组织模式
- 事务脚本模式（高度面向过程）
- 领域模型模式（面向对象设计）

### 领域驱动设计 DDD
DDD 是对面向对象设计的改进，是开发复杂业务逻辑的一种方法。子域概念有助于把应用程序分解为服务，每个服务都有自己的领域模型，这就避免了在单个应用程序全局范围内的领域模型问题。

**DDD 的基本元素**
- 实体 entity：具有持久化 ID 的对象
- 值对象 value object：作为值集合的对象
- 工厂 factory：负责实现对象创建逻辑的对象或方法，该逻辑过于复杂，无法由类的构造函数直接完成。
- 存储库 repository：用来访问持久化实体的对象，储存库也封装了访问数据库的底层机制。
- 服务 service：实现不属于实体或值对象的业务逻辑的对象。

**使用聚合模式设计领域模型**

**领域事件**
- 在命名领域事件时，我们往往选择动词的过去分词，这样的命名能够明确表达事件的一些属性。领域事件的每个属性都是原始值或值对象。
- 领域事件通常还具有元数据，例如事件 ID 和时间戳。


## 六、使用事件溯源开发业务逻辑
事件溯源是一种以事件为中心的技术，用于实现业务逻辑和聚合的持久化。聚合作为一系列事件存储在数据库中。每个事件代表聚合的状态变化。聚合的业务逻辑围绕生成和使用这些事件的要求而构建。

使用事件溯源开发业务逻辑：
- 传统持久化技术的问题 （ORM）
- 使用乐观锁处理并发更新
- 事件溯源和发布事件（轮询发布）
- 使用快照提升性能
- 幂等方式处理消息

**好处**
TODO

**弊端**
TODO


## 七、在微服务架构中实现查询
### 使用API组合模式进行查询
简单，应尽可能使用。工作原理是让拥有数据的服务的客户端负责调用服务，并组合服务返回的查询结果。

实现方式：API 组合器调用不同的服务，并组合服务返回的结果。

优点：
- 简单直观

缺点：
1. 增加了额外的开销
2. 带来可用性降低的风险
3. 缺乏事务数据一致性

### 使用CQRS模式
CQRS 隔离命令和查询：CQRS 是命令查询指责隔离（Command Query Responsibility Segregation）的简称，设计隔离或问题的分隔。

它将持久化数据模型和使用数据的模块分为两部分：命令端和查询端。
1. 命令端模块和数据模型实现创建、更新和删除操作（缩写为CUD，例如 HTTP POST、PUT 和 DELETE）。
2. 查询端模块和数据模型实现查询（HTTP GET）

查询端通过订阅命令端发布的事件，使其数据模型与命令端数据模型保持同步。

好处：
1. 在微服务架构中高效地实现查询
2. 高效地实现多种不同的查询类型
3. 在基于事件溯源技术的应用程序中实现查询
4. 更进一步地实现问题隔离

弊端：
1. 更加复杂的架构
2. 处理数据复制导致的延迟


## 八、外部API模式
直接访问服务的 API 会导致很多问题。客户端通过互联网完成 API 组合通常是不切实际的。缺乏封装使开发人员更难以更改服务分解和 API。服务有时会使用不适合防火墙之外的通信协议。所以就需要使用 API Gateway。

API Gateway 是一种服务，它是外部世界进入应用程序的入口点。它负责请求路由、API 组合和身份验证等各项功能。

API Gateway 功能：
- 请求路由
- API 组合
- 协议转换：REST 和 RPC
- 为每一个客户端提供它们专用的 API
- 实现边缘功能
  - 身份验证
  - 访问授权
  - 速率限制
  - 缓存
  - 指标收集
  - 请求日志

API Gateway 的架构：
1. API 层：由一个或多个独立的 API 模块组成，每个 API 模块都为特定客户端实现 API。
2. 公共层：实现共享功能，包括边缘功能，如身份验证。

好处：封装了应用程序的内部结构

弊端：它是一个必须开发、部署和管理的高可用组件，但存在成为开发瓶颈的风险。

API Gateway 的设计难题：
1. 性能和可扩展性
2. 使用响应式编程抽象编写可维护的代码
3. 处理局部故障
4. 成为应用程序架构中的好公民


## 九 & 十、微服务架构中的测试策略
### 自动化测试的四个阶段
1. 设置环境
2. 执行测试
3. 验证结果
4. 清理环境

### 测试的不同类型
单元测试：测试服务的一小部分，例如类

集成测试：验证服务是否可以与基础设施服务（如数据库）或其他应用程序服务进行交互

组件测试：单个服务的验收测试

端到端测试：整个应用程序的验收测试

### 测试金字塔
从低层到高层测试规模需要依次减少

单元测试 -> 集成测试 -> 组件测试 -> 端到端测试

### 使用测试象限进行分类
- 测试面向业务还是面向技术
- 测试的目标是协助开发还是寻找产品缺陷


## 十一、开发面向生产环境的微服务应用
### 安全性
- 是否验证：jwt token
- 访问授权：oauth2.0
- 审计
- 安全的进程间通信

### 可配置
- 基于推送的外部化配置：环境变量
- 基于拉取的外部化配置
  - 集中配置
  - 敏感数据的透明解密
  - 动态重新配置

### 设计可观测的服务
- 健康检查 API：Kubernetes health check
- 日志聚合：ELK
- 分布式跟踪：Zipkin、Jaeger
- 异常跟踪：Sentry
- 应用程序指标：Prometheus & Grafana
- 审核日志记录

### 使用微服务基底模式开发服务
微服务基底是一个框架或一组框架，可以处理许多问题
- 外部化配置
- 健康检查
- 应用程序指标
- 服务发现
- 断路器
- 分布式追踪

### 服务网格
概念：
- 把所有进出服务的网络流量通过一个网络层进行路由，这个网络层负责解决包括断路器、分布式追踪、服务发现、负载均衡和基于规则的流量路由等具有共性的需求。
- 使用服务网格后，微服务基底需要担负的责任就少了很多。
- 服务网格旨在解决开发中的各种共性问题。

常用技术：istio


## 十二：部署微服务应用
### 虚拟机
主要用来部署单体应用

### docker k8s 容器部署
**k8s**
- 主要功能
  - 资源管理
  - 调度
  - 服务调度
- 架构
  - 主节点
    - API 服务器：用于部署和管理服务的 REST API
    - Etcd：存储集群数据值的 NoSQL 数据库
    - 调度器：选择要运行的 Pod 的节点
    - 控制器管理：运行控制器，确保集群状态与预期状态匹配。
  - 普通节点
    - Kubelet：创建和管理节点上运行的 Pod
    - Kube-proxy：管理网络，包括跨 Pod 的负载均衡
    - Pods：应用程序服务
- 关键概念
  - Pod：Kubernetes 的基本部署单元
  - Deployment：Pod 的声明性规范，维护 Pod
  - Service：向应用程序服务的客户端提供的一个静态/稳定的网络地址，是基础设施提供的服务发现的一种形式。
  - ConfigMap：名称与值对的命名集合，用于定义一个或多个应用程序服务的外部化配置。

**服务网格 istio**
- 部署发布介绍
  - 部署：让服务开始在生产环境中运行
  - 发布：使最终用户可以使用（访问）服务
- 作用：将部署流程与发布流程分开
  - 将新版本部署到生成环境中，而不向其路由任何最终用户请求。
  - 在生产中进行测试。
  - 将其发布给少数最终用户。
  - 逐步将其发布给越来越多的用户，直到它处理所有生产流量为止。
  - 任何时候出现问题，请恢复旧版本，否则，一旦你确信新版本正常工作，请删除旧版本。
- 主要功能
  - 流量管理：包括服务发现、负载均衡、路由规则和断路器
  - 通信安全：使用传输层安全性（TLS）保护服务间通信
  - 遥测 Telemetry：捕获有关网络流量的指标并实施分布式跟踪
  - 策略执行：强制实施配额和费率限制

**部署流水线**
- 提交前测试阶段
- 提交测试阶段
- 集成测试阶段
- 组件测试阶段
- 部署阶段

### Serverless 部署
介绍：
- 使用公有云提供的 Serverless 部署机制部署服务。例如 AWS、阿里云的函数计算服务
- 阿里云函数计算是事件驱动的全托管计算服务。通过函数计算，您无需管理服务器等基础设施，只需编写代码并上传。函数计算会为您准备好计算资源，以弹性、可靠的方式运行您的代码，并提供日志查询、性能监控、报警等功能。

使用函数计算的好处：
- 有许多公有云服务可供集成
- 消除许多系统管理任务
- 弹性
- 基于使用情况定价

使用函数计算的弊端：
- 长尾延迟
- 基于有限事件与请求的编程模型


## 十三、微服务架构的重构策略
### 需要考虑的问题
- 将单体应用重构为微服务架构的时机
  - 交付缓慢
  - 充满故障的软件交付
  - 可扩展性差
- 绞杀单体应用
  - 逐步重构单体应用程序，而不是推倒重来。
  - 绞杀者应用程序由与单体应用程序结合使用的微服务组成，随着时机的推移，单体应用程序实现的功能数量会缩小，直到完全小时或者变成另一个微服务。
- 尽早并且频繁地体现出价值
- 尽可能少对单体做出修改
- 部署基础设施：这还为时尚早
  - 开始先进行简单的部署方式

### 重构策略
- 将新功能实现为服务
  - 具体还要看新功能是否能实现为新服务，避免与单体服务打交道。
- 隔离表现层与后端
  - 将表现层与业务逻辑和数据访问层分开
- 从单体提取业务能力到独立服务中
- 拆解领域模型 DDD
  - TODO 难点
- 重构数据库
  - 将表从单体的数据库移动到服务的数据库
- 复制数据以避免更广泛的更改
- 确定提取何种服务以及何时提取
  - 根据提取应用程序模块获得的预期收益，对应用程序的模块进行排名
  - 提取服务的好处
    - 加速开发
    - 解决性能、可扩展性和可靠性问题
    - 允许提取其他一些服务

### 设计服务与单体的协作方式
- 设计集成胶水：使服务能够与单体进行协作，集成胶水由服务和单体中的代码组成。
- 维持服务和单体之间数据一致性：事务
- 处理身份验证和访问授权：修改单体的登录处理程序以生成包含安全令牌的 cookie，然后由 API Gateway 转发给服务。

### 将新功能实现为服务

### 从单体中提取功能到独立服务

## 参考
[微服务架构设计模式](https://book.douban.com/subject/33425123/)
