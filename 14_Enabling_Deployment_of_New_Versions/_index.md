---
title: "14. 支持新版本的部署"
anchor: "14_Enabling_Deployment_of_New_Versions"
weight: 1400
rank: "h1"
---

QUIC版本1在基本规范中没有规定一种版本协商机制，但是《[QUIC-VERSION-NEGOTIATION](https://datatracker.ietf.org/doc/html/draft-ietf-quic-version-negotiation-10)》提出的扩展提供了一项具有兼容性的版本协商机制。

该方法使用了一项三阶段的部署机制，支持在大型服务器部署中对多版本进行渐进式的版本发布与实验。
在此方法中，部署中的所有服务器都必须先接受使用了新版本的连接（阶段1），再宣布对该版本的支持（阶段2），最后仅在宣布支持的新版本已完成部署后才对新版本进行认证（阶段3）。

有关细节详见《[QUIC-VERSION-NEGOTIATION](https://datatracker.ietf.org/doc/html/draft-ietf-quic-version-negotiation-10)》的[第5章](https://datatracker.ietf.org/doc/html/draft-ietf-quic-version-negotiation-10#section-5)。
