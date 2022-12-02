---
title: "12. 服务质量（QoS）与差分服务码点（DSCP）"
anchor: "12_Quality_of_Service_QoS_and_Diffserv_Code_Point_DSCP"
weight: 1200
rank: "h1"
---

QUIC, as defined in [QUIC], has a single congestion controller and recovery handler. This design assumes that all packets of a QUIC connection, or at least with the same 5-tuple {dest addr, source addr, protocol, dest port, source port}, that have the same Diffserv Code Point (DSCP) [RFC2475] will receive similar network treatment since feedback about loss or delay of each packet is used as input to the congestion controller. Therefore, packets belonging to the same connection should use a single DSCP. Section 5.1 of [RFC7657] provides a discussion of Diffserv interactions with datagram transport protocols [RFC7657] (in this respect, the interactions with QUIC resemble those of Stream Control Transmission Protocol (SCTP)).

正如《[QUIC](../RFC9000_Chinese_Simplified)》中定义的那样，QUIC具有单一的拥塞控制器和恢复处理器。
这种设计假定，只要在关于各个数据包的丢失或延迟反馈全都被用作拥塞控制器的输入的条件下，同一条QUIC连接上的或至少五元组（目标地址、源地址、协议、目标端口、源端口）一致的，且具有相同差分服务码点（DSCP，详见《[RFC2475](https://www.rfc-editor.org/info/rfc2475)》）的所有数据包就都能获得相似的网络条件待遇。
因此，归属于同一条连接的数据包应该使用相同的DSCP。
《[RFC7657](https://www.rfc-editor.org/info/rfc7657)》的[第5.1章](https://www.rfc-editor.org/rfc/rfc7657#section-5.1)中讨论了差分服务与数据报传输协议（详见《[RFC7657](https://www.rfc-editor.org/info/rfc7657)》）的交互方式（在这一方面，与QUIC的交互和与流控制传输协议（SCTP）的交互类似）。

When multiplexing multiple flows over a single QUIC connection, the selected DSCP value should be the one associated with the highest priority requested for all multiplexed flows.

当多路流量复用同一条QUIC连接时，所选的DSCP值应该就是关联着为所有流量请求的DSCP中优先级最高的那个值。

If differential network treatment is desired, e.g., by the use of different DSCPs, multiple QUIC connections to the same server may be used. In general, it is recommended to minimize the number of QUIC connections to the same server to avoid increased overhead and, more importantly, competing congestion control.

如果希望获得差异化的网络条件待遇，例如，要使用不同的DSCP，那么可以使用与同一服务器间的多条QUIC连接。
通常来说，推荐将与同一服务器间的QUIC连接数降至最低，以降低开销，以及，更重要的一点是，与拥塞控制机制竞争。

As in other uses of Diffserv, when a packet enters a network segment that does not support the DSCP value, this could result in the connection not receiving the network treatment it expects. The DSCP value in this packet could also be remarked as the packet travels along the network path, changing the requested treatment.

正如差分服务的其他使用场景那样，当数据包进入不支持DSCP值的网段时，连接可能无法得到它所期望的网络条件。
数据包中的DSCP还可能随着数据包在网络路径上的传输而被重新标记，使得所请求的网络条件待遇发生变化。
