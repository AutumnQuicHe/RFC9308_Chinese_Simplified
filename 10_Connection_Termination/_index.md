---
title: "10. 连接终止"
anchor: "10_Connection_Termination"
weight: 1000
rank: "h1"
---

QUIC connections are terminated in one of three ways: implicit idle timeout, explicit immediate close, or explicit stateless reset.

QUIC的连接可能以三种方式之一终止：隐式的空闲超时、显式的立即关闭，或明确的无状态重置。

QUIC does not provide any mechanism for graceful connection termination; applications using QUIC can define their own graceful termination process (see, for example, Section 5.2 of [QUIC-HTTP]).

QUIC并没有提供任何优雅地终止连接的机制；使用QUIC的应用可以定义自己的优雅终止流程（作为样例，详见《[QUIC-HTTP](../RFC9114_Chinese_Simplified)》的[第5.2章](../RFC9114_Chinese_Simplified/#5.2_Connection_Shutdown)）。

QUIC idle timeout is enabled via transport parameters. The client and server announce a timeout period, and the effective value for the connection is the minimum of the two values. After the timeout period elapses, the connection is silently closed. An application therefore should be able to configure its own maximum value, as well as have access to the computed minimum value for this connection. An application may adjust the maximum idle timeout for new connections based on the number of open or expected connections since shorter timeout values may free up resources more quickly.

QUIC的空闲超时是通过传输参数来启用的。客户端和服务器各声明一个超时时长，随后，对于此连接来说的有效值就是两个值中的较小的那个。一旦经过了超时时长，连接就会被静默地关闭。因此应用应该有能力配置其最大超时时长，并能够访问此连接上的超时计算值。应用可以基于开放的或期望的连接数为新连接调整其最大空闲超时时长，因为更短的超时值可以更频繁地触发资源回收。

Application data exchanged on streams or in datagrams defers the QUIC idle timeout. Applications that provide their own keep-alive mechanisms will therefore keep a QUIC connection alive. Applications that do not provide their own keep-alive can use transport-layer mechanisms (see Section 10.1.2 of [QUIC] and Section 3.2). However, QUIC implementation interfaces for controlling such transport behavior can vary, affecting the robustness of such approaches.

在流或数据报中通信的应用数据会推迟QUIC的空闲超时。因此具有自己的`keep-alive`机制的应用可以保持QUIC连接的活跃。并未提供自己的`keep-alive`机制的应用则可以利用传输层的一些机制（详见[第3.2章](#3.2_Session_Resumption_versus_Keep_Alive)和《[QUIC](../RFC9000_Chinese_Simplified)》的[第10.1.2章](../RFC9000_Chinese_Simplified/#10.1.2_Deferring_Idle_Timeout)）。然而，不同的QUIC实现中，控制这类传输层行为的接口可能不同，影响这类方法的健壮性。

An immediate close is signaled by a CONNECTION_CLOSE frame (see Section 6). Immediate close causes all streams to become immediately closed, which may affect applications; see Section 4.5.

立即关闭是通过**连接关闭帧**（详见[第6章](#6_Error_Handling)）来发送信号的。立即关闭将使得所有流都被立即关闭，这可能会对应用产生影响；详见[第4.5章](#4.5_Stream_Limit_Commitments)。

A stateless reset is an option of last resort for an endpoint that does not have access to connection state. Receiving a stateless reset is an indication of an unrecoverable error distinct from connection errors in that there is no application-layer information provided.

无状态重置是一个无法访问连接状态的终端的最终手段。接收到无状态重置表明出现了并非连接错误的某种无法恢复的错误，且其中不存在应用层信息。
