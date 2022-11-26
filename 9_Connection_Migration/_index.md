---
title: "9. 连接迁移"
anchor: "9_Connection_Migration"
weight: 900
rank: "h1"
---

QUIC supports connection migration by the client. If the client's IP address changes, a QUIC endpoint can still associate packets with an existing transport connection using the Destination Connection ID field (see Section 11) in the QUIC header. This supports cases where the address information changes, such as NAT rebinding, the intentional change of the local interface, the expiration of a temporary IPv6 address [RFC8981], or the indication from the server of a preferred address (Section 9.6 of [QUIC]).

Use of a non-zero-length connection ID for the server is strongly recommended if any clients are or could be behind a NAT. A non-zero-length connection ID is also strongly recommended when active migration is supported. If a connection is intentionally migrated to a new path, a new connection ID is used to minimize linkability by network observers. The other QUIC endpoint uses the connection ID to link different addresses to the same connection and entity if a non-zero-length connection ID is provided.

The base specification of QUIC version 1 only supports the use of a single network path at a time, which enables failover use cases. Path validation is required so that endpoints validate paths before use to avoid address spoofing attacks. Path validation takes at least one RTT, and congestion control will also be reset after path migration. Therefore, migration usually has a performance impact.

QUIC probing packets, which can be sent on multiple paths at once, are used to perform address validation as well as measure path characteristics. Probing packets cannot carry application data but likely contain padding frames. Endpoints can use information about their receipt as input to congestion control for that path. Applications could use information learned from probing to inform a decision to switch paths.

Only the client can actively migrate in version 1 of QUIC. However, servers can indicate during the handshake that they prefer to transfer the connection to a different address after the handshake. For instance, this could be used to move from an address that is shared by multiple servers to an address that is unique to the server instance. The server can provide an IPv4 and an IPv6 address in a transport parameter during the TLS handshake, and the client can select between the two if both are provided. See Section 9.6 of [QUIC].
