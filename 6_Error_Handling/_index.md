---
title: "6. 错误处理"
anchor: "6_Error_Handling"
weight: 600
rank: "h1"
---

QUIC recommends that endpoints signal any detected errors to the peer. Errors can occur at the transport layer and the application layer. Transport errors, such as a protocol violation, affect the entire connection. Applications that use QUIC can define their own error detection and signaling (see, for example, Section 8 of [QUIC-HTTP]). Application errors can affect an entire connection or a single stream.

QUIC推荐终端为所有检测到的错误都向对端发出信号。错误可以在传输层和应用层发生。传输层错误，例如协议违反，会影响整条连接。使用QUIC的应用可以定义自己的错误检测与信号机制（作为样例，详见《[QUIC-HTTP]()》的[第8章]()）。应用层错误可以影响整条连接或单条流。

QUIC defines an error code space that is used for error handling at the transport layer. QUIC encourages endpoints to use the most specific code, although any applicable code is permitted, including generic ones.

QUIC定义了用于传输层错误处理的错误码空间。QUIC鼓励终端使用最为准确的错误码，不过允许使用任何适用的错误码，包括通用的那些。

Applications using QUIC define an error code space that is independent of QUIC or other applications (see, for example, Section 8.1 of [QUIC-HTTP]). The values in an application error code space can be reused across connection-level and stream-level errors.

使用QUIC的应用定义的错误码空间独立于QUIC或其他应用（作为样例，详见《[QUIC-HTTP]()》的[第8.1章]()）。应用层错误码空间中的值可以在连接级错误和流级错误间复用。

Connection errors lead to connection termination. They are signaled using a CONNECTION_CLOSE frame, which contains an error code and a reason field that can be zero length. Different types of CONNECTION_CLOSE frames are used to signal transport and application errors.

连接错误将引发连接终止。它们是使用**连接关闭帧**来发出信号的，其中包含着一个错误码和一个可以为空的原因字段。传输层错误和应用层错误是使用不同类型的**连接关闭帧**来发出信号的。

Stream errors lead to stream termination. These are signaled using STOP_SENDING or RESET_STREAM frames, which contain only an error code.

流错误将引发流的终止。它们是使用**停止发送帧**或**流重置帧**来发出信号的，其中只包含一个错误码。
