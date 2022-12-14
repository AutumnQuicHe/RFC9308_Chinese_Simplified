---
title: "5. 分包与延迟"
anchor: "5_Packetization_and_Latency"
weight: 500
rank: "h1"
---

QUIC暴露了一个可以向应用提供多条流的接口；然而，应用通常无法控制用这些流传输的数据与帧的映射方式，也无法控制这些帧被打包进数据包的方式。

默认情况下，许多实现都会向各个QUIC数据包中尽可能多地填充来自一条或多条流的**流帧**，从而最小化带宽消耗和计算成本（详见《[QUIC](../RFC9000_Chinese_Simplified)》的[第13章](../RFC9000_Chinese_Simplified/#13_Packetization_and_Reliability)）。
如果没有足够的可用数据来填充数据包，那么实现可能等待一段较短的延迟，牺牲延迟以优化带宽效率。
该延迟可以是预配置的，或是基于应用发送模式的观察结果而动态调整得到的。

如果应用对低延迟有要求，又只有小块的数据需要发送，那么向QUIC提示应该尽快发送所有数据的做法可能是有价值的。
作为替代方案，它也可以向QUIC提供一个建议的延迟值，指出在将帧打包进数据包前可以等待多久。

类似地，应用一般无法控制通信线路上的QUIC数据包长度。
QUIC提供了添加**填充帧**，从而任意扩大数据包尺寸，的能力。
QUIC使用填充来确保在握手期间（详见《[QUIC](../RFC9000_Chinese_Simplified)》的[第14.1章](../RFC9000_Chinese_Simplified/#14.1_Initial_Datagram_Size)）、连接迁移后的路径验证（详见《[QUIC](../RFC9000_Chinese_Simplified)》的[第8.2章](../RFC9000_Chinese_Simplified/#8.2_Path_Validation)）时，以及进行数据报分包层PMTU发现（DPLPMTUD，详见《[QUIC](../RFC9000_Chinese_Simplified)》的[第14.3章](../RFC9000_Chinese_Simplified/#14.3_Datagram_Packetization_Layer_PMTU_Discovery)）时，路径有能力传输至少为某个特定尺寸大小的数据报。

应用还可以使用填充来抑制被发送数据的信息泄露。
QUIC实现可以暴露一个接口，允许应用层指定如何进行填充。
