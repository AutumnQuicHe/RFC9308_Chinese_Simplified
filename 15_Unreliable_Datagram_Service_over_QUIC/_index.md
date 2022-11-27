---
title: "15. 基于QUIC的不可靠数据报服务"
anchor: "15_Unreliable_Datagram_Service_over_QUIC"
weight: 1500
rank: "h1"
---

[RFC9221] specifies a QUIC extension to enable sending and receiving unreliable datagrams over QUIC. Unlike operating directly over UDP, applications that use the QUIC datagram service do not need to implement their own congestion control, per [RFC8085], as QUIC datagrams are congestion controlled.

《[RFC9221]()》中定义的QUIC扩展能够支持基于QUIC发送和接收不可靠的数据报。不同于直接基于UDP运行，使用QUIC数据报服务的应用不需要按照《[RFC8085]()》实现自己的拥塞控制协议，因为QUIC数据报都会受到拥塞控制。

QUIC datagrams are not flow controlled, and as such data chunks may be dropped if the receiver is overloaded. While the reliable transmission service of QUIC provides a stream-based interface to send and receive data in order over multiple QUIC streams, the datagram service has an unordered message-based interface. If needed, an application-layer framing can be used on top to allow separate flows of unreliable datagrams to be multiplexed on one QUIC connection.

QUIC数据报不会受到流量控制，因为接收方在过载时会丢弃这类数据块。尽管可靠的QUIC传输服务提供了基于流的接口以经由多条QUIC流来发送和接收数据，但是数据报服务的接口是基于无序消息的。若有需要，可以在它之上使用应用层帧结构来支持多路不可靠的数据报流量复用同一条QUIC连接。
