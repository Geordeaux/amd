---
id: overview
title: Overview
---

分布式追踪是用来将不同工作单元的信息关联起来的技术，通常是在不同进程或主机中执行的，以便理解分布式事务中的整个事件链。分布式追踪可让开发人员在大型服务架构中视觉化调用流程。它对理解序列化、平行和延迟来源会很有价值。

[OpenTracing](https://opentracing.io/) 是 CNCF 提出的分布式追踪的标准，它提供用厂商中立的 API，并提供 Go、Java、JavaScript、Python、Ruby、PHP、Objective-C、C++ 和 C# 这九种语言的库。

目前支持 Tracer 包括 Zipkin、Skywalking、Jaeger 等，支持的框架包括 gRPC、MOTAN、django、Flask、Sharding-JDBC 等。

分布式追踪代表了一个事务或者流程在系统中的执行过程。在 OpenTracing 标准中，一个trace（调用链）是由多个 Span 组成的一个有向无环图，每一个 Span 代表事务的一个执行节点或者片段，每一个 Span 都是有名字并且有起止时间的。

分布式追踪中的每个组件都有一个或者多个 Span。例如，在一个常规的 RPC 调用过程中，OpenTracing 推荐在 RPC 的客户端和服务端，至少各有一个 Span，用于记录 RPC 调用的客户端和服务端信息。


## 基本术语

1. **Trace**: 通常指一次完整的调用链,代表了一个事务或者流程在系统中的执行过程.
1. **Span**: 每个 trace 都由一系列 Span 组成，一个 span 可以理解为两个微服务之间的调用。
根据 OpenTracing 的规格约定，每个 Span 都要包含以下状态：
    - 操作名称：可以是访问的一个 URL。必填。例如 localhost:8808/。
    - 起/止时间戳：也可以使用起始时间和持续时间来表示。必填。例如 1540273832696773。
    - Tags：一组键值对集合，Semantic Conventions 有一些常用约定。必填。例如 http.protocol。
    - Logs：一组键值对集合，用于记录调用日志。可选填。
    - SpanContext：在进程间通信时携带的 span 信息，指整个 trace。

Span 有两个主要的关系：
1. **ChildOf**：表示父子关系。父 Span 需要等待子 Span 结束才能结束
1. **FollowsFrom**：表示兄弟关系。两者各自的生命周期是独立的