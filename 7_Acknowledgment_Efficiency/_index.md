---
title: "7. 确认频率"
anchor: "7_Acknowledgment_Efficiency"
weight: 700
rank: "h1"
---

未经扩展的QUIC版本1采用一份源自TCP的确认策略（详见《[QUIC](../RFC9000_Chinese_Simplified)》的[第13.2章](../RFC9000_Chinese_Simplified/#13.2_Generating_Acknowledgments)）。
也就是说它推荐对所有数据包进行确认。
然而，创建和处理QUIC确认会同时消耗发送方和接收方的资源。
确认还会提高转发成本，并增加链路负载，这会影响部分类型的网络的性能。
通过改用降低确认频率的替代策略，应用或许有能力影响综合性能。
《[QUIC-ACK-FREQUENCY](https://datatracker.ietf.org/doc/html/draft-ietf-quic-ack-frequency-02)》中描述了一种能够提示所需的确认延迟的扩展，并讨论了使用场景及其对拥塞控制和恢复的影响。
