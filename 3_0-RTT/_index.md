---
title: "3. 0-RTT"
anchor: "3_0-RTT"
weight: 300
rank: "h1"
---

QUIC在连接建立时允许发送0-RTT数据。
尽管基于TCP的TLS 1.3中存在同样的特性，但是0-RTT为使用QUIC的应用同时带来了机遇与挑战。

从使用0-RTT的应用视角来看，在连接建立期间支持0-RTT的传输协议与不支持的协议有着本质区别。
在先关闭连接再打开新连接的开销与试图保持连接开放的开销之间权衡时的思路是不同的；详见《[第3.2章](#3.2_Session_Resumption_versus_Keep_Alive)》。

应用应该谨慎选择要不要支持0-RTT，因为0-RTT带来了重放攻击的风险。
支持0-RTT的应用协议应该准备一份使用说明，在其中描述它可以安全发送的信息类型。
对于HTTP来说，《[HTTP-REPLAY](https://www.rfc-editor.org/info/rfc8470)》中描述了其使用说明。
