---
title: "8. 端口选择与应用终端发现"
anchor: "8_Port_Selection_and_Application_Endpoint_Discovery"
weight: 800
rank: "h1"
---

In general, port numbers serve two purposes: "first, they provide a demultiplexing identifier to differentiate transport sessions between the same pair of endpoints, and second, they may also identify the application protocol and associated service to which processes connect" (Section 3 of [RFC6335]). The assumption that an application can be identified in the network based on the port number is less true today due to encapsulation and mechanisms for dynamic port assignments, as noted in [RFC6335].

通常来说，端口号具有两种目的：“首先，它们提供了解多路复用的标识符，从而区分同一对终端间的传输层会话；第二，它们还标识着应用协议和与进程连接着的关联服务”（详见《[RFC6335](https://www.rfc-editor.org/info/rfc6335)》的[第3章](https://www.rfc-editor.org/rfc/rfc6335#section-3)）。时至今日，动态端口分配的机制与封装的出现使得可以基于端口号来辨识网络应用的假设不再准确，详见《[RFC6335](https://www.rfc-editor.org/info/rfc6335)》。

As QUIC is a general-purpose transport protocol, there are no requirements that servers use a particular UDP port for QUIC. For an application with a fallback to TCP that does not already have an alternate mapping to UDP, it is usually appropriate to register (if necessary) and use the UDP port number corresponding to the TCP port already registered for the application. For example, the default port for HTTP/3 [QUIC-HTTP] is UDP port 443, analogous to HTTP/1.1 or HTTP/2 over TLS over TCP.

由于QUIC是一份通用传输协议，因此不要求服务器为QUIC使用特定的UDP端口。对于能够回退至TCP但是尚未具有备选的UDP端口号映射的应用，通常来说，注册并使用与应用已注册的TCP端口号一致的UDP端口号的做法是合适的。例如，HTTP/3（详见《[QUIC-HTTP](../RFC9114_Chinese_Simplified)》）的默认端口是UDP端口`443`，与使用基于TCP的TLS的HTTP/1.1或HTTP/2类似。

Given the prevalence of the assumption in network management practice that a port number maps unambiguously to an application, the use of ports that cannot easily be mapped to a registered service name might lead to blocking or other changes to the forwarding behavior by network elements such as firewalls that use the port number for application identification.

鉴于网络管理实践中普遍存在端口号明确映射到应用程序的假设，使用无法轻易映射到已注册服务名称的端口可能会导致转发行为受到网络元素，例如使用端口号进行应用程序识别的防火墙，的阻塞或其他更改。

Applications could define an alternate endpoint discovery mechanism to allow the usage of ports other than the default. For example, HTTP/3 (Sections 3.2 and 3.3 of [QUIC-HTTP]) specifies the use of HTTP Alternative Services [RFC7838] for an HTTP origin to advertise the availability of an equivalent HTTP/3 endpoint on a certain UDP port by using "h3" as the Application-Layer Protocol Negotiation (ALPN) [RFC7301] token.

应用可以定义备选的终端发现机制以允许使用并非默认端口的端口号。例如，HTTP/3（详见《[QUIC-HTTP](../RFC9114_Chinese_Simplified)》的[第3.2章](../RFC9114_Chinese_Simplified/#3.2_Connection Establishment)和[第3.3章](../RFC9114_Chinese_Simplified/#3.3_Connection_Reuse)）规定了使用HTTP替代服务（详见《[RFC7838](https://www.rfc-editor.org/info/rfc7838)》），在某HTTP源上以`h3`作为应用层协议协商（ALPN，详见《[RFC7301](https://www.rfc-editor.org/info/rfc7301)》）词汇的方式宣告与其等价的HTTP/3终端正位于某个特定UDP端口上。

ALPN permits the client and server to negotiate which of several protocols will be used on a given connection. Therefore, multiple applications might be supported on a single UDP port based on the ALPN token offered. Applications using QUIC are required to register an ALPN token for use in the TLS handshake.

ALPN允许客户端和服务器协商在给定连接上究竟使用哪份协议。因此，基于提供的ALPN词汇，在单个UDP端口上可以支持数个应用。为了用于TLS握手，使用QUIC的协议必须注册一个ALPN词汇。

As QUIC version 1 deferred defining a complete version negotiation mechanism, HTTP/3 requires QUIC version 1 and defines the ALPN token ("h3") to only apply to that version. So far, no single approach has been selected for managing the use of different QUIC versions, neither in HTTP/3 nor in general. Application protocols that use QUIC need to consider how the protocol will manage different QUIC versions. Decisions for those protocols might be informed by choices made by other protocols, like HTTP/3.

由于QUIC版本1将完整的版本协商机制的定义推迟了，所以HTTP/3指定使用QUIC版本1，且定义了ALPN词汇（`h3`）以专门适用于该版本。至今为止，无论是在HTTP/3中还是在一般用途中，都尚未指定任何单一方法来应对多QUIC版本的使用。使用QUIC的应用协议应该考虑协议如何应对不同的QUIC版本。这类协议在此问题上的决策或许可以从其他协议，如HTTP/3，的判断中获得启发。
