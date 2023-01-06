---
title: "8. 端口选择与应用终端发现"
anchor: "8_Port_Selection_and_Application_Endpoint_Discovery"
weight: 800
rank: "h1"
---

通常来说，端口号具有两种目的：“首先，它们提供了解多路复用的标识符，从而区分同一对终端间的传输层会话；第二，它们还标识着应用协议和与进程连接着的关联服务”（详见《[RFC6335](https://www.rfc-editor.org/info/rfc6335)》的[第3章](https://www.rfc-editor.org/rfc/rfc6335#section-3)）。
时至今日，动态端口分配的机制与封装的出现使得可以基于端口号来辨识网络应用的假设不再准确，详见《[RFC6335](https://www.rfc-editor.org/info/rfc6335)》。

由于QUIC是一份通用传输协议，因此不要求服务器为QUIC使用特定的UDP端口。
对于能够回退至TCP但是尚未具有备选的UDP端口号映射的应用，通常来说，注册并使用与应用已注册的TCP端口号一致的UDP端口号的做法是合适的。
例如，HTTP/3（详见《[QUIC-HTTP](../RFC9114_Chinese_Simplified)》）的默认端口是UDP端口`443`，与使用基于TCP的TLS的HTTP/1.1或HTTP/2类似。

鉴于网络管理实践中普遍存在端口号明确映射到应用程序的假设，使用无法轻易映射到已注册服务名称的端口可能会导致转发行为受到网络元素，例如使用端口号进行应用程序识别的防火墙，的阻塞或其他更改。

应用可以定义备选的终端发现机制以允许使用并非默认端口的端口号。
例如，HTTP/3（详见《[QUIC-HTTP](../RFC9114_Chinese_Simplified)》的[第3.2章](../RFC9114_Chinese_Simplified/#3.2_Connection Establishment)和[第3.3章](../RFC9114_Chinese_Simplified/#3.3_Connection_Reuse)）规定了使用HTTP替代服务（详见《[RFC7838](https://www.rfc-editor.org/info/rfc7838)》），在某HTTP源上以`h3`作为应用层协议协商（ALPN，详见《[RFC7301](https://www.rfc-editor.org/info/rfc7301)》）词汇的方式宣告与其等价的HTTP/3终端正位于某个特定UDP端口上。

ALPN允许客户端和服务器协商在给定连接上究竟使用哪份协议。
因此，基于提供的ALPN词汇，在单个UDP端口上可以支持数个应用。
为了用于TLS握手，使用QUIC的协议必须注册一个ALPN词汇。

由于QUIC版本1将完整的版本协商机制的定义推迟了，所以HTTP/3指定使用QUIC版本1，且定义了ALPN词汇（`h3`）以专门适用于该版本。
至今为止，无论是在HTTP/3中还是在一般用途中，都尚未指定任何单一方法来应对多QUIC版本的使用。
使用QUIC的应用协议应该考虑协议如何应对不同的QUIC版本。
这类协议在此问题上的决策或许可以从其他协议，如HTTP/3，的判断中获得启发。
