---
title:
---
## 链路层服务
### Framing
封装上层数据形成帧。
![](../../img/Pasted%20image%2020240501155454.png)
### Link Access
- 协调多设备链接
- MAC地址用以表示源与目的地
- 半双工/全双工决定是否能够双向传输
#### 多重访问控制(Multiple Access Control, MAC)
使用**分布式算法**解决如何在一个共享广播信道上避免多个节点"Speaking at once"的问题，即避免Collision。决定那个节点可以传输。
三种种类的技术：
1. 信道划分
2. 轮流
3. 随机接入(Random Access)
	1. 允许collision，但需要recovery
### Reliable delivery over the link
- 在low bit-error link上少用（e.g. fiber)
- 高错误率链路：无线
- Flow Control：Pacing between adjacent sending and receiving nodes
### Error Detection and Correction
- 处理信号衰减或噪声引起的错误
- 接受者检测错误并丢弃错误帧/通知发送者重传
