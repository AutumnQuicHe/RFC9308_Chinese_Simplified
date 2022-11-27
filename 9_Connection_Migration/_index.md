---
title: "9. 连接迁移"
anchor: "9_Connection_Migration"
weight: 900
rank: "h1"
---

QUIC supports connection migration by the client. If the client's IP address changes, a QUIC endpoint can still associate packets with an existing transport connection using the Destination Connection ID field (see Section 11) in the QUIC header. This supports cases where the address information changes, such as NAT rebinding, the intentional change of the local interface, the expiration of a temporary IPv6 address [RFC8981], or the indication from the server of a preferred address (Section 9.6 of [QUIC]).

QUIC支持由客户端发起的连接迁移。如果客户端的IP地址发生变化，那么QUIC终端仍然可以使用QUIC数据包头部中的目标连接ID字段（详见[第11章](#11_Information_Exposure_and_the_Connection_ID)）来将数据包关联到一条现存的传输层连接上。该特性能够支持地址信息变化，例如NAT重绑定、本地接口更改、临时IPv6地址（详见《[RFC8981](https://www.rfc-editor.org/info/rfc8981)》）过期，以及来自服务器的首选地址指示（详见《[QUIC](../RFC9000_Chinese_Simplified)》的[第9.6章](../RFC9000_Chinese_Simplified/#9.6_Server's_Preferred_Address)）等场景。

Use of a non-zero-length connection ID for the server is strongly recommended if any clients are or could be behind a NAT. A non-zero-length connection ID is also strongly recommended when active migration is supported. If a connection is intentionally migrated to a new path, a new connection ID is used to minimize linkability by network observers. The other QUIC endpoint uses the connection ID to link different addresses to the same connection and entity if a non-zero-length connection ID is provided.

如果存在或可能存在位于NAT后方的客户端，那么强烈推荐为服务器使用非空连接ID的做法。当支持活跃迁移时，也强烈推荐使用非空的连接ID。如果主动将连接迁移至新路径，那么使用新连接ID可以针对网络上的观察者将可关联性降至最低。如果使用了非空的连接ID，那么其他的QUIC终端可以使用连接ID来将不同地址关联至同一条连接。

The base specification of QUIC version 1 only supports the use of a single network path at a time, which enables failover use cases. Path validation is required so that endpoints validate paths before use to avoid address spoofing attacks. Path validation takes at least one RTT, and congestion control will also be reset after path migration. Therefore, migration usually has a performance impact.

QUIC版本1的基本规范仅支持在同一时间使用一条网络路径，这能够支持故障切换的使用场景。要求终端在使用路径前先进行路径验证是为了避免地址伪造攻击。路径验证需要至少一轮RTT，同时拥塞控制状态会在路径迁移完成后被重置。因此，迁移对性能通常具有影响。

QUIC probing packets, which can be sent on multiple paths at once, are used to perform address validation as well as measure path characteristics. Probing packets cannot carry application data but likely contain padding frames. Endpoints can use information about their receipt as input to congestion control for that path. Applications could use information learned from probing to inform a decision to switch paths.

QUIC的探测数据包被用于进行路径验证和测量路径特征，这种数据包可以同时在多条路径上发送。探测数据包不能传递应用数据，但是可能携带着填充帧。终端可以将接收到的探测数据包及其信息作为针对该路径的拥塞控制机制的输入。应用可以使用从探测中了解到的信息来决定要不要切换路径。

Only the client can actively migrate in version 1 of QUIC. However, servers can indicate during the handshake that they prefer to transfer the connection to a different address after the handshake. For instance, this could be used to move from an address that is shared by multiple servers to an address that is unique to the server instance. The server can provide an IPv4 and an IPv6 address in a transport parameter during the TLS handshake, and the client can select between the two if both are provided. See Section 9.6 of [QUIC].

在QUIC版本1中，只有客户端可以发起迁移。不过，服务器可以在握手期间表示出它倾向于在握手完成后将连接转移到另一个地址的意愿。作为范例，可以利用该机制来从一个与其他服务器共享的地址移动到服务器实例专属的地址上。服务器可以在TLS握手期间的传输参数中提供一个IPv4地址和一个IPv6地址，客户端则可以在两者均被提供时在其中选择一个。详见《[QUIC](../RFC9000_Chinese_Simplified)》的[第9.6章](../RFC9000_Chinese_Simplified/#9.6_Server's_Preferred_Address)。
