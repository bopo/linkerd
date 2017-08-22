# 服务发现

运行多业务应用程序固有的大部分复杂性源于服务发现。不幸的是，随着应用程序的复杂性和规模的增加，服务发现变得难以避免。linkerd 是明确设计以减少这种复杂性，通过：

1. 抽象出底层服务发现机制的细节
2. 提供升级路径，允许选择适当的服务发现端点
3. 鼓励基于生产系统中使用服务发现的经验而来的最佳实践。

linkerd 抽象服务发现机制，以简单统一的方式对待它们：作为简单的数据存储，能够将具体的名称解析为一组地址

这种极简主义的交互，结合 linkerd 的路由规则，提供了强大的控制力，同时降低了复杂性。 例如，linkerd 能够使用多个服务发现端点并在它们之间表示优先级和故障转移。

linkerd 具有将服务发现视为权威或咨询的能力。默认情况下配置授权服务发现，但可以通过 linkerd 的 enableProbation 负载均衡器配置切换。在权威模式下，当被冲服务发现中删除时，linkerd 将停止向实例发送流量。

咨询服务发现不适用于所有环境，因此默认情况下不启用。然而，在许多环境中，咨询服务发现有助于防止服务发现后端的故障，包括完全中断和丢失或无效的数据。在咨询模式下，如果端点仍然可用并处理请求，则 linkerd 可能会在从服务发现中删除后将流量发送到地址。

在权威或咨询模式下，实例不需要从服务发现中删除，以停止接收流量。如果实例只是停止接受请求，则 linkerd 的负载均衡算法被设计为可以优雅地处理变得不健康或消失的实例。

服务发现中的查找由 [dtab规则](https://linkerd.io/in-depth/dtabs/) 控制，即这些查找包括请求路由逻辑的一部分。