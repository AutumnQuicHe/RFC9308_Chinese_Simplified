---
title: "17. 关于安全性的考量"
anchor: "17_Security_Considerations"
weight: 1700
rank: "h1"
---

详见《[QUIC](../RFC9000_Chinese_Simplified)》和《[QUIC-TLS](../RFC9001_Chinese_Simplified)》中关于安全性的考量；底层传输协议的安全性与使用QUIC的应用密切相关。
在部署和使用QUIC时，还要考虑在《[QUIC-TLS](../RFC9001_Chinese_Simplified)》中讨论的可关联性、重放攻击，以及随机化等议题。

更进一步地，迁移至新地址会向服务器暴露客户端所用地址间的关联，还有可能在连接ID未改变或流量因其他方式而被关联的情况下向路径上的设备暴露该关联性。
当支持迁移时，需要考虑以上因素以保护用户隐私。

应用开发者应该注意，他们应该确保在因为网络拦截UDP而无法使用QUIC的情况下所用的任何回退都具有与QUIC一致的安全属性。
如果做不到这一点，就应该断开连接，以使得显式地令应用判断要不要回退至安全性较低的替代方案。
详见[第2章](#2_The_Necessity_of_Fallback)。

除此之外，《[QUIC-HTTP](../RFC9114_Chinese_Simplified)》中还提供了关于HTTP安全性的考量。
不过，对于形如跨协议攻击、流量分析、填充，以及迁移等主题的讨论还有可能与其他使用QUIC的应用相关。
