---
title: "2. 回退的必要性"
anchor: "2_The_Necessity_of_Fallback"
weight: 200
rank: "h1"
---

QUIC uses UDP as a substrate. This enables userspace implementation and permits traversal of network middleboxes (including NAT) without requiring updates to existing network infrastructure.

QUIC将UDP用作基础协议。
这么做能支持用户空间中的实现且无需更新现有网络基础设施就能允许中间设备进行网络穿透（其中包括NAT）。

Measurement studies have shown between 3% [Trammell16] and 5% [Swett16] of networks block all UDP traffic, though there is little evidence of other forms of systematic disadvantage to UDP traffic compared to TCP [Edeline16]. This blocking implies that all applications running on top of QUIC must either be prepared to accept connectivity failure on such networks or be engineered to fall back to some other transport protocol. In the case of HTTP, this fallback is TLS over TCP.

尽管相比TCP，几乎没有证据表明UDP流量还具有其他形式的系统性缺陷，但是测量类的研究表明，3%（详见《[Trammell16](https://ripe72.ripe.net/wp-content/uploads/presentations/86-atlas-udpdiff.pdf)》）至5%（详见《[Swett16](https://www.ietf.org/proceedings/96/slides/slides-96-quic-3.pdf)》）的网络会拦截一切UDP流量。
这种拦截行为意味着所有基于QUIC运行的应用都必须要么准备好接受这类网络中的连接故障，要么具有能够回退到其他传输协议的架构。
在HTTP的场景中，回退方案就是基于TCP的TLS。

The IETF Transport Services (TAPS) specifications [TAPS-ARCH] describe a system with a common API for multiple protocols. This is particularly relevant for QUIC as it addresses the implications of fallback among multiple protocols.

IETF传输服务（TAPS）规范（详见《[TAPS-ARCH](https://datatracker.ietf.org/doc/html/draft-ietf-taps-arch-14)》）中描述的系统具有面向多种协议的通用API。
这与QUIC尤其相关，因为它解决了在不同协议间回退所带来的影响。

Specifically, fallback to insecure protocols or to weaker versions of secure protocols needs to be avoided. In general, an application that implements fallback needs to consider the security consequences. A fallback to TCP and TLS exposes control information to modification and manipulation in the network. Additionally, downgrades to TLS versions older than 1.3, which is used in QUIC version 1, might result in significantly weaker cryptographic protection. For example, the results of protocol negotiation [RFC7301] only have confidentiality protection if TLS 1.3 is used.

需要强调的是，应该避免回退至不安全的协议或同协议在安全性上较弱的版本。
一般来说，实现了回退的应用应该考虑其安全性上的影响。
回退至TCP和TLS的做法将使得网络设备有能力修改与操纵控制信息。
除此之外，降级至低于1.3的TLS版本，即QUIC版本1中使用的TLS版本，可能造成加密保护效果的严重弱化。
例如，只有在使用了TLS 1.3时，协议协商（详见《[RFC7301](https://www.rfc-editor.org/info/rfc7301)》）的结果才具有可信度保护。

These applications must operate, perhaps with impaired functionality, in the absence of features provided by QUIC not present in the fallback protocol. For fallback to TLS over TCP, the most obvious difference is that TCP does not provide stream multiplexing, and therefore stream multiplexing would need to be implemented in the application layer if needed. Further, TCP implementations and network paths often do not support the TCP Fast Open (TFO) option [RFC7413], which enables sending of payload data together with the first control packet of a new connection as also provided by 0-RTT session resumption in QUIC. Note that there is some evidence of middleboxes blocking SYN data even if TFO was successfully negotiated (see [PaaschNanog]). And even if Fast Open successfully operates end to end, it is limited to a single packet of TLS handshake and application data, unlike QUIC 0-RTT.

这类应用必须有能力在回退协议缺少QUIC所提供的特性或具有功能缺陷的情况下运行。
对于回退至基于TCP的TLS的做法，最显著的区别是TCP并不提供流的多路复用特性，因此应用层应该在必要时实现流的多路复用。
除此之外，TCP实现和网络路径经常不支持“TCP快速打开”（TFO）选项，该选项像QUIC中提供的0-RTT会话恢复机制一样，允许在新连接上将载荷数据与首个控制数据包一起发送。
注意，目前有证据表明某些中间设备即便在成功协商TFO后也会拦截SYN数据（详见《[PaaschNanog](https://www.nanog.org/sites/default/files/Paasch_Network_Support.pdf)》）。
即便两端间的快速打开正常运作，它也仅作用于单个包含TLS握手与应用数据的数据包，比不上QUIC的0-RTT。

Moreover, while encryption (in this case TLS) is inseparably integrated with QUIC, TLS negotiation over TCP can be blocked. If TLS over TCP cannot be supported, the connection should be aborted, and the application then ought to present a suitable prompt to the user that secure communication is unavailable.

更进一步地，尽管加密（即TLS）是紧密集成在QUIC内部的，但是基于TCP的TLS协商却可以被拦截。
如果不支持基于TCP的TLS，那么应该中止连接，且随后应用应该向用户做出合适的提醒，告知其目前无法进行安全通信。

In summary, any fallback mechanism is likely to impose a degradation of performance and can degrade security; however, fallback must not silently violate the application's expectation of confidentiality or integrity of its payload data.

综上所述，所有回退机制都有可能带来性能上的恶化和安全性上的削弱；不过，回退绝不可以静默地违背应用对其载荷数据可信度与完整性的预期。
