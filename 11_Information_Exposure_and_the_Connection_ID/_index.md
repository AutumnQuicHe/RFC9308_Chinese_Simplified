---
title: "11. 信息暴露与连接ID"
anchor: "11_Information_Exposure_and_the_Connection_ID"
weight: 1100
rank: "h1"
---

QUIC exposes some information to the network in the unencrypted part of the header either before the encryption context is established or because the information is intended to be used by the network. For more information on manageability of QUIC, see [QUIC-MANAGEABILITY]. QUIC has a long header that exposes some additional information (the version and the source connection ID), while the short header exposes only the destination connection ID. In QUIC version 1, the long header is used during connection establishment, while the short header is used for data transmission in an established connection.

QUIC在头部未经加密的部分中向网络暴露了一些信息，这么做要么是因为尚未建立加密上下文，要么是有意将这些信息提供给网络使用。有关更多QUIC可管理性的信息，详见《[QUIC可管理性](../RFC9312_Chinese_Simplified)》。QUIC的长包头会暴露更多信息（版本与源连接ID），而短包头只会暴露目标连接ID。在QUIC版本1中，连接建立期间使用的是长包头，而在已建立的连接中传输数据时使用的是短包头。

The connection ID can be zero length. Zero-length connection IDs can be chosen on each endpoint individually and on any packet except the first packets sent by clients during connection establishment.

连接ID可以为零长度。各个终端都能独立选择要不要使用零长度连接ID，除了客户端在连接建立期间发送的首个数据包外的所有数据包都可以使用零长度连接ID。

An endpoint that selects a zero-length connection ID will receive packets with a zero-length destination connection ID. The endpoint needs to use other information, such as the source and destination IP address and port number to identify which connection is referred to. This could mean that the endpoint is unable to match datagrams to connections successfully if these values change, making the connection effectively unable to survive NAT rebinding or migrate to a new path.

选择使用零长度连接ID的终端将接收到零长度目标连接ID的数据包。终端应该使用其他信息，例如源IP地址和目标IP地址及端口号来分辨它归属于哪条连接。这意味着一旦这些值发生变化，终端就可能无法将数据报与连接成功匹配，使得连接无法经受NAT重绑定或迁移至新路径。
