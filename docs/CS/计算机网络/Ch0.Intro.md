## Internet基本概念

### 什么是Internet?

因特网是一个世界范围的计算机网络，即它是一个互联了遍及全世界数十亿计算设备的网络 。

#### Component

Connected computing devices, links, routers

#### Service

为分布式apps提供可靠/best effort的数据传输，保证时延与吞吐量

#### Protocol

协议(protocol)定义了在两个或多个通信实体之间交换的报文的格式和顺序，以及报文发送和／或接收一条报文或其他事件所采取的动作 。 因特网（更一般地说是计算机网络）广泛地使用了协议 。

## 如何连接到Internet
### 组成
#### 网络边缘
端系统、服务器
#### 网络接入
##### 家庭接入
Dialup via modem，DSL: digital subscriber line，HFC: hybrid fiber coax
##### 公司接入
以太网, WiFi
##### 无线接入
共享无线媒介、无线局域网（WiFi）、广域无线接入（3G,4G:LTE）

##### 物理媒介
双绞铜线，同轴电缆(coaxial cable)，光缆

#### 网络核心

交换机、路由器以及连接他们的链路

## 如何通过Internet传输数据
### 交换方式
#### 电路交换
- **为每条链接资源预留**
- 优点:性能可控，交换快捷简单
- 缺点:电路建立较复杂，专用资源在突发数据传输时效率低，电路建立会增加时延，对特定switch的依赖性强
#### 分组交换
- **每个packet独立处理**
- 优点:对网络资源的有效利用，实现简单，鲁棒性高
- 缺点:performance难以预测，需要buffer管理和拥塞控制

在电路交换网络中，在端系统间通信会话期间，预留了端系统间沿路径通信所需要的资源（缓存，链路传输速率）在分组交换网络中．这些资源则不是预留的

在发送方能够发送信息之前，该网络必须在发送方和 接收方之间建立一 条连接。这是一个名副其实的连接，因为此时沿着发送方和接收方间路径上的交换机都将为该连接维护连接状态 。


### 统计多路复用
- 所有用户不会同时使用网络，因此可以许诺给每个用户大于线路承载总合的带宽
- Works well except for the extreme cases

#### 虚电路

![](../../img/Pasted%20image%2020240501145122.png)

![](../../img/Pasted%20image%2020240501145150.png)
### 协议体系结构

Internet过于复杂：大量应用程序和协议、各种end system

显式结构使得复杂系统的各个部分能够被识别并确定其相互关系。模块化使得系统的管理和升级变得简单。

#### OSI
应用层、表示层、会话层、传输层、网络层、链路层、物理层
![](../../img/Pasted%20image%2020240501150108.png)

![image-20230614172507831](file://C:/Users/x/AppData/Roaming/Typora/typora-user-images/image-20230614172507831.png?lastModify=1714544548)

- 应用层：message 
	- Means for applications to access OSI environment
- 传输层：**segment** 
	- Exchange of data between end systems
- 网络层：**datagram** 
	- Transfers packets across multiple links / multiple networks
- 链路层：**frame** 
	- Transfers frames across direct connections
- 物理层：**bit** 
	- Transfers bits across link
![](../../img/Pasted%20image%2020240501150750.png)

## 性能评估

### Delay

Delay = Transmission Delay(传输时延) + Propagation Delay(传播时延) + Queuing Delay(排队时延) + Processing Delay

- **传播时延**
	- How long does it take to move one bit from one end of a link to the other?
- **传输时延**
	- How long does it take to **push all the bits of a packet into a link**?
- **排队时延**
	- How long does a packet have to sit in a buffer before it is processed?
	- **平均排队数目L=平均到达速率A * 平均等待时间W**

### 吞吐量
At what rate is the destination receiving data from the source
![](../../img/Pasted%20image%2020240501152455.png)
![](../../img/Pasted%20image%2020240501152525.png)
