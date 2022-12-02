---
title: "13. 版本与加密握手的使用"
anchor: "13_Use_of_Versions_and_Cryptographic_Handshake"
weight: 1300
rank: "h1"
---

Versioning in QUIC may change the protocol's behavior completely, except for the meaning of a few header fields that have been declared to be invariant [QUIC-INVARIANTS]. A version of QUIC with a higher version number will not necessarily provide a better service but might simply provide a different feature set. As such, an application needs to be able to select which versions of QUIC it wants to use.

QUIC的版本变化有可能完全改变协议行为，除了那些声明为不变量的少数头部字段的含义（详见《[QUIC不变量](../RFC8999_Chinese_Simplified)》）。
版本号更大的QUIC版本不一定会提供更优质的服务，而是可能只是提供一份不同的特性集。
因此，应用应该有能力选择它想要使用的QUIC版本。

A new version could use an encryption scheme other than TLS 1.3 or higher. [QUIC] specifies requirements for the cryptographic handshake as currently realized by TLS 1.3 and described in a separate specification [QUIC-TLS]. This split is performed to enable lightweight versioning with different cryptographic handshakes.

新版本可能使用并非TLS 1.3的加密方案，或更高的TLS版本。
《[QUIC](../RFC9000_Chinese_Simplified)》为加密握手制定了一些目前由TLS 1.3实现的要求，并在单独的规范《[QUIC-TLS](../RFC9001_Chinese_Simplified)》中进行了描述。
分为两份文档是为了支持对不同的加密握手方案制定轻量化的版本。

The "QUIC Versions" registry established in [QUIC] allows for provisional registrations for experimentation. Registration, also of experimental versions, is important to avoid collision. Experimental versions should not be used long-term or registered as permanent to minimize the risk of fingerprinting based on the version number.

《[QUIC](../RFC9000_Chinese_Simplified)》中建立的“QUIC版本”注册表允许存在实验性的临时注册项。
对实验性版本的注册对于避免冲突十分重要。
实验性的版本不应该被长期使用，或应该作为永久注册项从而最小化基于版本号的指纹识别风险。
