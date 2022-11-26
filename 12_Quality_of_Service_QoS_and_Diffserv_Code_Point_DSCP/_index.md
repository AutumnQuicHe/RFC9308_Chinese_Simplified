---
title: "12. 服务质量（QoS）与差分服务码点（DSCP）"
anchor: "12_Quality_of_Service_QoS_and_Diffserv_Code_Point_DSCP"
weight: 1200
rank: "h1"
---

QUIC, as defined in [QUIC], has a single congestion controller and recovery handler. This design assumes that all packets of a QUIC connection, or at least with the same 5-tuple {dest addr, source addr, protocol, dest port, source port}, that have the same Diffserv Code Point (DSCP) [RFC2475] will receive similar network treatment since feedback about loss or delay of each packet is used as input to the congestion controller. Therefore, packets belonging to the same connection should use a single DSCP. Section 5.1 of [RFC7657] provides a discussion of Diffserv interactions with datagram transport protocols [RFC7657] (in this respect, the interactions with QUIC resemble those of Stream Control Transmission Protocol (SCTP)).

When multiplexing multiple flows over a single QUIC connection, the selected DSCP value should be the one associated with the highest priority requested for all multiplexed flows.

If differential network treatment is desired, e.g., by the use of different DSCPs, multiple QUIC connections to the same server may be used. In general, it is recommended to minimize the number of QUIC connections to the same server to avoid increased overhead and, more importantly, competing congestion control.

As in other uses of Diffserv, when a packet enters a network segment that does not support the DSCP value, this could result in the connection not receiving the network treatment it expects. The DSCP value in this packet could also be remarked as the packet travels along the network path, changing the requested treatment.
