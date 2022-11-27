---
title: "1. 引言"
anchor: "1_Introduction"
weight: 100
rank: "h1"
---

QUIC [QUIC] is a new transport protocol providing a number of advanced features. While initially designed for the HTTP use case, it provides capabilities that can be used with a much wider variety of applications. QUIC is encapsulated in UDP. QUIC version 1 integrates TLS 1.3 [TLS13] to encrypt all payload data and most control information. The version of HTTP that uses QUIC is known as HTTP/3 [QUIC-HTTP].

QUIC（详见《[QUIC](../RFC9000_Chinese_Simplified)》）是一种全新的传输协议，它提供了诸多高级特性。尽管起初是为HTTP的使用场景来设计的，但它提供的能力可以被广泛应用在其他应用中。QUIC是用UDP封装的。QUIC版本1中集成了TLS 1.3（详见《[TLS13](https://www.rfc-editor.org/info/rfc8446)》），从而加密所有载荷数据与绝大多数控制信息。基于QUIC的HTTP版本被称为HTTP/3（详见《[QUIC-HTTP](../RFC9114_Chinese_Simplified)》）。

This document provides guidance for application developers who want to use the QUIC protocol without implementing it on their own. This includes general guidance for applications operating over HTTP/3 or directly over QUIC.

本文档为想要使用QUIC协议的应用开发者提供了指导，而无需开发者亲自实现QUIC协议。其中包括对基于HTTP/3或直接运行于QUIC之上的应用的通用指导。

In the following sections, we discuss specific caveats to QUIC's applicability and issues that application developers must consider when using QUIC as a transport for their applications.

在后续的章节中，我们讨论了针对QUIC适用范围的警告和应用开发者在将QUIC用作其应用的传输方式时必须考虑的问题。
