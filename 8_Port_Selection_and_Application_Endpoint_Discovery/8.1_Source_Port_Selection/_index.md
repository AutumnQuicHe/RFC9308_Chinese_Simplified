---
title: "8.1. 源端口的选择"
anchor: "8.1_Source_Port_Selection"
weight: 810
rank: "h2"
---

某些UDP协议容易受到反射攻击，在这类攻击中，攻击者有能力将流量定向至第三方，引发拒绝服务。
例如，与以下源端口关联的应用已知容易受到反射攻击，其常常是因为服务器的错误配置：

* 端口`53` - DNS （详见《[RFC1034](https://www.rfc-editor.org/info/rfc1034)》）

* 端口`123` - NTP （详见《[RFC5905](https://www.rfc-editor.org/info/rfc5905)》）

* 端口`1900` - SSDP （详见《[SSDP](https://openconnectivity.org/upnp-specs/UPnP-arch-DeviceArchitecture-v2.0-20200417.pdf)》）

* 端口`5353` - mDNS （详见《[RFC6762](https://www.rfc-editor.org/info/rfc6762)》）

* 端口`11211` - memcache

为了避免处理大量数据包所带来的开销，服务或许会拦截与已知易受反射攻击的协议关联的源端口。
然而，这项实践在客户端上具有负面影响——它不仅需要建立新连接，在某些情况下还可能造成客户端在一段时间内避免用QUIC来连接该服务并降级到非UDP协议（详见[第2章](#2_The_Necessity_of_Fallback)）。

因此，鼓励客户端上的实现避免使用与已知易受反射攻击的协议关联的源端口。
注意，遵循《[RFC6335](https://www.rfc-editor.org/info/rfc6335)》中给出的客户端实现通用指导的做法会产生避免使用这些端口的效果。
还要注意，其他源端口仍有可能成为反射攻击的目标。
