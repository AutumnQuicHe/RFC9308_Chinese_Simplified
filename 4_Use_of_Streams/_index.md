---
title: "4. 流的使用"
anchor: "4_Use_of_Streams"
weight: 400
rank: "h1"
---

QUIC's stream multiplexing feature allows applications to run multiple streams over a single connection without head-of-line blocking between streams. Stream data is carried within frames where one QUIC packet on the wire can carry one or multiple stream frames.

QUIC流的多路复用特性允许应用在单条连接之上运行多条流，而不会引入流与流之间的队头阻塞。流数据是用帧来传递的，通信线路上的一个QUIC数据包能够携带一个或多个流帧。

Streams can be unidirectional or bidirectional, and a stream may be initiated either by client or server. Only the initiator of a unidirectional stream can send data on it.

流可以是单向的或双向的，且流可以由客户端和服务器中的任意一方发起。对于单向流，只有其发起方才能发送数据。

Streams and connections can each carry a maximum of 262-1 bytes in each direction due to encoding limitations on stream offsets and connection flow control limits. In the presently unlikely event that this limit is reached by an application, a new connection would need to be established.

由于流数据偏移字段的编码限制和连接上的流量控制限制，流和连接分别只能在单个方向上传递至多<code>2<sup>62</sup>-1</code>字节的数据。在应用触及该上限这样一种不太可能发生的情况下，应该建立新连接。

Streams can be independently opened and closed, gracefully or abruptly. An application can gracefully close the egress direction of a stream by instructing QUIC to send a FIN bit in a STREAM frame. It cannot gracefully close the ingress direction without a peer-generated FIN, much like in TCP. However, an endpoint can abruptly close the egress direction or request that its peer abruptly close the ingress direction; these actions are fully independent of each other.

流可以被，优雅地或强行地，独立打开和关闭。应用可以通过令QUIC在**流帧**中发送`FIN`置位的方式，优雅地将某条流的发送方向关闭。只要对端没有发送`FIN`，它就不能优雅地关闭流的接收方向，这和TCP很像。然而，终端可以强行关闭发送方向，或要求对端强行关闭接收方向；这些操作完全不依赖于对端的意愿。

QUIC does not provide an interface for exceptional handling of any stream. If a stream that is critical for an application is closed, the application can generate error messages on the application layer to inform the other end and/or the higher layer, which can eventually terminate the QUIC connection.

QUIC并不提供对任何流进行异常处理的接口。如果一条对于应用来说很关键的流被关闭了，那么应用可以在应用层创建错误信息，以告知对端及/或其更高层，后者最终有能力终止QUIC连接。

Mapping of application data to streams is application specific and described for HTTP/3 in [QUIC-HTTP]. There are a few general principles to apply when designing an application's use of streams:

将应用数据映射至流的方式视应用的不同而不同，《[QUIC-HTTP]()》中描述了HTTP/3的映射。在设计某应用对流的使用方式时，存在一些适用的通用准则：

* A single stream provides ordering. If the application requires certain data to be received in order, that data should be sent on the same stream. There is no guarantee of transmission, reception, or delivery order across streams.

* 单条流内的数据是有序的。如果应用要求按顺序接收特定数据，那么就应该在同一条流上发送这些数据。在不同的流间，不保证传输、接收和送达顺序。

* Multiple streams provide concurrency. Data that can be processed independently, and therefore would suffer from head-of-line blocking if forced to be received in order, should be transmitted over separate streams.

* 多条流间的数据是并行的。可以被独立处理，且在强制接收顺序的情况下会引发队头阻塞的数据应该经由多条单独的流来传输。

* Streams can provide message orientation and allow messages to be canceled. If one message is mapped to a single stream, resetting the stream to expire an unacknowledged message can be used to emulate partial reliability for that message.

* 流支持消息定向，且允许取消消息。如果某条消息被映射至某条流上，那么重置该流就能使得某未得到确认的消息超时，利用这一点可以模拟该消息的部分可靠性。

If a QUIC receiver has opened the maximum allowed concurrent streams, and the sender indicates that more streams are needed, it does not automatically lead to an increase of the maximum number of streams by the receiver. Therefore, an application should consider the maximum number of allowed, currently open, and currently used streams when determining how to map data to streams.

如果QUIC的接收方打开的并发流数量达到了上限，发送方又表示需要更多的流，那么这不会导致接收方提高流的数量上限。因此，应用应该在决定如何将数据映射至流上时将允许的流数量上限、当前打开的流数量和当前使用的流数量纳入考量。

QUIC assigns a numerical identifier, called the stream ID, to each stream. While the relationship between these identifiers and stream types is clearly defined in version 1 of QUIC, future versions might change this relationship for various reasons. QUIC implementations should expose the properties of each stream (which endpoint initiated the stream, whether the stream is unidirectional or bidirectional, the stream ID used for the stream); applications should query for these properties rather than attempting to infer them from the stream ID.

QUIC向每条流分配一个数字标识符，称之为流ID。尽管QUIC版本1中清晰地定义了这些标识符与流类型间的关系。但是将来的版本可能出于各种理由更改此关系。QUIC实现应该将各条流的属性暴露出来（哪个终端发起了流、流是单向的还是双向的、流的ID等）；应用应该查询这些属性，而不是尝试从流ID中推断它们。

The method of allocating stream identifiers to streams opened by the application might vary between transport implementations. Therefore, an application should not assume a particular stream ID will be assigned to a stream that has not yet been allocated. For example, HTTP/3 uses stream IDs to refer to streams that have already been opened but makes no assumptions about future stream IDs or the way in which they are assigned (see Section 6 of [QUIC-HTTP]).

向应用打开的流分配流标识符的方法可能因传输协议的实现不同而不同。因此，应用不应该假设某个特定的流ID会被分配给某条尚未得到分配的流。例如，HTTP/3使用流ID来引用已打开的流，但对于将来的流ID或其分配方式不做任何假设（详见《[QUIC-HTTP]()》的[第6章]()）。
