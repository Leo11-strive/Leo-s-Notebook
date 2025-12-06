* Network Layer Design Issues
* very important
* routing algorithm
* internetworking
* software defined networking
* the network layer in the internet
![[image-678.png]]

* host 有一个数据包，发送给路由器，store and forward
![[image-679.png]]
* 笔记本，手机是终端
* 传输双方通过多跳连接
* 路由算法
* 服务和路由器实现无关
* 向上一层隐藏一节
* connection oriented/connection-less
![[image-680.png]]
* datagram subnet
* virtual-circuit subnet
![[image-681.png]]
* table: 匹配 ip 的 destination address
> Destination-》下一个 router 的映射
![[image-682.png]]
* 面向连接
* 前面两列：进入数据包
> Source address 到底是谁，1 的意思是连接号，连接id
* 后面两列：输出
> 就是 input output 不匹配是因为视野的问题，但是整体是一个连接，id 是组成连接的连接器，VC number，每个链接在路由器里面有储存
* datagram 的路由器可以改路径，virtual circuit 不行，一个路由坏了就会出现问题
![[image-683.png]]
* store and forward
> 保存然后发送


## Routing algorithm
* pattern, technique 的区分
> 模式与技术
* 最最常见的：unicast：点对点
* broadcast 广播：one to all
* multicast：one to many
* anycast：one to any
> 访问 DNS 服务器，选一个即可
* collection：
> All to one
> Used in sensor networks

* 路由算法决定数据包如何转发
![[image-711.png]]
* 路由器两个工作：
> Forwarding：找匹配项，转发
> Routing: 如何决定路由表
* Desiable properties
![[image-712.png]]
> 算法不断收敛，不能到处摇摆

![[image-713.png]]
* A, B, C，X 除了 max flow，还要考虑公平性
> 最大化最小流

* 分类：非自适应算法，不会随着网络的变化而变化
* 自适应算法：自适应变化
* 最优化原则：
![[image-714.png]]
* 最短路径会构成树
![[image-715.png]]
* 跳数是比较好的，path 的指标是链路的加和得到的；delay 也是比较好的，delay 可以加起来

* 用 Dijkstra 算法计算最短路径
![[image-716.png]]

![[image-717.png]]

* Distance vector routing
* Bellman Ford 算法，不断更新迭代
* the drawback of DV，好的消息传播快，坏的消息传播慢（大小）
![[image-718.png]]
* 第二次交换的时候，n 次消息交换，所有节点知道这些信息
* 如果相反，AB 变烂了，因此 B 会选择绕路(甚至会是错误的)，因为大小
![[image-719.png]]
* 毒性逆转的问题（Poisoned reverse）

## Link state routing
* 本质上是让每个节点知道整个网络的拓扑
* 如何让每个节点知道整个网络的拓扑？
![[image-720.png]]
* 先了解每个邻居的信息
* 构建数据包，就是前面两步的信息
* 接受这些信息
* 得到所有的信息
![[image-721.png]]
* 邻居不能太多
* a better way
> 引入人工伪造的节点
* Pseudonode
> 人工引入，如何发送 LS packat？选择一个 router 起到这个节点的实际应用
![[image-724.png]]
* 通过人共引入，简化拓扑
![[image-725.png]]
* 这是计算 delay 的方法
![[image-722.png]]
* IP 地址，sequence number/age（是否是 outdated）
* 节点重构：重大事件，隔一段时间

* Distributing the link state packet
* 最基础的想法就是 flooding
> 改进：
> 只处理新的信息
![[image-726.png]]

![[image-723.png]]
* discard

* step 5: computing the new routes
> Dijkstra algorithm
![[image-727.png]]

![[image-728.png]]
* hierarchical routing
> 层次化路由
> 因为 IP 地址很多，routing tables 会越来越大
* 引入层次化
![[image-729.png]]
* 只知道自己的信息
* multilevel 也是的
![[image-730.png]]
* 如何使用呢？
> 会有 full table，如果用 hierarchical table 呢？自己的拓扑信息必须都要知道，只要知道区域分类即可
* 是否存在 panelty？
* 从 1 到 5 牺牲最优性
![[image-731.png]]


![[image-757.png]]
* DV：迭代，比较慢

## Broadcast
* sink tree: 网络里面一些点的完整最短路径
![[image-758.png]]
* 不是包含所有的地方，一个包可以复用
* 洪泛：其他的线全部都要转发
* sink-tree：汇集树：其他所有节点到这个节点的最短路径，利用这个数据结构，传输最少的 pkts
> 某种意义上来说是 optimal，一共发送了多少的数据包
* reverse path forwarding
![[image-759.png]]
* 按照这个树传输即可，树上有多少边，发多少个数据包
* 如果做 sink tree based algorithm, 必须每个节点都有共识且都储存这个最短路径信息
![[image-760.png]]
* 如果每个节点使用 LS，那这个事情可行
* 如果使用 DV，每个节点不知道整网的拓扑的，不太可行

### Reverse path forwarding
![[image-761.png]]

* 先检测进来的路线是否在我的最佳路径上
> 比如从 J 节点进来，look for 这是不是最佳路径，如果是，所有线转发，如果不是，那就不转发（既然传进来不是最佳路径，那么就是一个叶子节点，不需要传），但是会有 extra overhead

* reverse path forwarding: works with DV

## Multicasting
* 组播
* to send messages to well-defined groups that are numerically large in size but small compared to the network as whole.
![[image-787.png]]

![[image-788.png]]

![[image-789.png]]

### Anycast Routing
* a packet is delivered to the nearest member of a group. Schemes
that find these paths are called anycast routing.

![[image-790.png]]
* 一堆服务器公用一个地址，选择最短的那个即可

![[image-791.png]]
Step 1：Register care-of address（向家乡报个“临时住址”）

当笔记本连到外地的网络时，会得到一个 **care-of address**。  
然后它会发一个 **注册消息** 给 Home agent：

> “我原来的 home address 是 H，现在我在外地，临时地址是 C（care-of address）。”

Home agent 把这个映射记在表里：  
`H → C`

这样以后只要有发往 H 的包，它就知道应该“转寄到 C”。

---
Step 2：Sender → home address（别人仍然按“家庭地址”发）

Sender 完全不知道笔记本去了哪里，它只知道对方的 **home address H**。

于是 Sender 按正常 IP 路由，把包发给 H。  
路由器一跳一跳转发，最终包会到达 **home agent 所在的网络**，被 home agent 收到。

> 注意：Sender 眼里，这次发送就是标准的 unicast，到一个普通 IP（H）。

---

Step 3：Home agent → mobile host（tunnel to care-of address）

Home agent 收到发往 H 的包后，查表发现：

> “哦，这家伙现在的 care-of address 是 C，不在家里。”

于是它做两件事：

1. **封装（encapsulation）**：
    
    - 把原来的 IP 包当作“payload”；
        
    - 外面再套一层新的 IP 头：
        
        - 源地址：Home agent
            
        - 目的地址：care-of address C  
            这就形成一个“隧道包”（tunnel）。
            
2. **沿着普通路由，把这个新包发到 C 所在的网络**。
    

到了外地网络，对应的外地路由器（可能是 foreign agent，或者直接就是移动主机）：

- 把外层 IP 头剥掉（decapsulation），
    
- 把里面原始、目的地址仍为 H 的那个 IP 包交给 mobile host。
    

这样，虽然 IP 层看到的目的地址是 H，  
但通过“**Home agent + 隧道**”的方式，包被“曲线送达”到了移动主机当前所在的网络。

这就是图上的 **“3: Tunnel to care of address”**。

---

Step 4：Mobile host → Sender（正常回包）

移动主机收到数据后，需要回一个应答给 Sender：

- 它可以直接用普通 IP 把包发回 Sender（目的地址 = Sender，源地址一般仍填自己的 **home address H**，以保持对上层的“地址不变”幻觉）；
    
- 路由上看，这就是从西部到东部的一条正常单播路径。
    

图上写的是 **“4: Reply to sender”**。

---

Step 5：Sender 后续发包（继续经过 home agent + 隧道）

接下来 Sender 继续给 H 发后续包：

- 这些包还是会先走到 Home agent（因为路由表里“去 H 就走东部那条路”）；
    
- Home agent 仍然按照 `H → C` 的映射，把它们**封装后隧道到 care-of address**；
    
- 移动主机再解封、接收。
    

图上这一步标的是 **“5: Tunnel to care of address”**，其实是和 Step 3 一样的动作，只是重复进行。


## Routing in Ad Hoc Networks
* The network topology may be changing all the time
* **AODV**: Ad hoc On-demand Distance Vector. The most popular routing algorithm for Ad Hoc Networks.

![[image-793.png]]


### Routing in Wireless Sensor Networks
* **WSN**: Wireless Sensor Networks
* **CTP**: Collection Tree Protocol is a popular routing protocol for WSN. **RPL** (IPv6 Routing Protocol for LLNs, RFC 6550) is a newer version that inherits many ideas from CTP.
![[image-794.png]]

## Internetworking
![[image-795.png]]

![[image-796.png]]

* To route a packet
> A typical internet packet starts out on its LAN addressed to the local multiprotocol router.
![[image-797.png]]

* tunneling
![[image-798.png]]

![[image-799.png]]

![[image-800.png]]

* Transparent fragmentation
* Nontransparent fragmentation
![[image-801.png]]
* **_Transparent_** is being used to mean hidden in the sense of things taking place automatically behind scenes (i.e. without user of the code or the program having to interact).


![[image-802.png]]

* 一个 packet number，第一个 fragment 和最后一个 fragment
* fragment
![[image-803.png]]

* Path MTU discovery
![[image-804.png]]


## Software Defined Networking
![[image-805.png]]

## SDN
* Internet network layer: historically implemented via distributed, **per-router control approach***
* 很多都是个体路由器，不同协议，实现不开源，不同的操作系统。
* 一个**封闭的一体化黑盒**
![[image-807.png]]

### Per-router control plane
* Individual routing algorithm components in each and every router interact in the control plane
![[image-806.png]]
![[image-808.png]]

* SDN 使用远程计算机计算数据并且安装到 routers 中

![[image-809.png]]
* 更加灵活

## Key characteristics of SDN
![[image-810.png]]

* SDN architecture
![[image-811.png]]
* Data-plane switches
> fast, simple, commodity switches implementing generalized data-plane forwarding in hardware
* flow (forwarding) table computed, installed under controller supervision
* API for table-based switch control(e.g., OpenFlow) defines what is controllable, what is not
* protocol for communicating with controller

### Flow table abstraction
* flow：由报文头部的一组字段共同定义的一类报文。
![[image-812.png]]

![[image-813.png]]

![[image-814.png]]

## OpenFLow
![[image-815.png]]

![[image-816.png]]

* maintain network state information
* interacts with network control applications “above” via northbound API
* implemented as distributed system for performance, scalability, fault-tolerance, robustness


### Network-control apps
* “brains” of control: implement control functions using lower-level services, API provided by SDN controller
* can be provided by 3rd party: distinct from routing vendor, or SDN controller

## Components of SDN controller
![[image-817.png]]

![[image-818.png]]

![[image-819.png]]


![[image-820.png]]

![[image-821.png]]

![[image-844.png]]


## IPv 4
* 4 个字节，$2^{32}$ 
* IPv 4 和 IPv 6 共存
![[image-845.png]]
* IP 数据包：header 和 text
* **Version**: to keep track of which version of the protocol the datagram belongs to.
* **IHL**: to tell how long the header is, in 32-bit words. 5 <= IHL <= 15.
* **Type of service** (past): 3 for priority, 3 for Delay, Throughput, and Reliability, and 2 unused.
* **Differentiated services** (now): 6 for service class, 2 for congestion
![[image-846.png]]

* **Identification**: datagram ID. All fragments belong to same ID
* **DF**: Don't Fragment
* **MF**: More Fragment. The flag is set for all fragments except the last
* **Fragment offset**: to tell where in the current datagram this fragment belongs. All fragments except the last one in a datagram must be a multiple of 8 bytes, the elementary fragment unit. Since 13 bits are provided, there is a maximum of 8192 fragments per datagram, supporting a maximum packet length up to 65536, the limit of the _Total_ _length_ field.
![[image-847.png]]
* Header checksum：以太网使用的校验码是 CRC 32，只对头部进行校验
* **Source and destination address**: to indicate the network number and host number.
![[image-848.png]]

![[image-849.png]]

![[image-850.png]]

![[image-851.png]]

![[image-852.png]]

* 这些前缀只是区分网段

* IP Addresses: Classless Inter-Domain Routing (CIDR)
![[image-853.png]]

![[image-854.png]]

![[image-855.png]]

* IP Addresses: Classful and Special Addressing
![[image-856.png]]

![[image-857.png]]

### Network Address Translation
* Addressing
> Permanent addresses: too few
> Temporary addresses (DHCP): large users

![[image-858.png]]

![[image-859.png]]

* NAT 的防火墙
> 内部是内网，IP 地址都是私网地址，之后接到外面就是公网了，它们都有在公网一个固定的 IP 地址
* 内网发送公网
> 只能通过 nat
* 这个NAT会知道这个数据包的来源（内网）和发送的端口，然后就会转换成自己的端口号和 IP 地址
* 目的 IP 地址就是 NAT 的地址
* NAT 的模式解决了 IP 地址不够用的问题，但是
> 违背了 IP 协议的模型
> 一个 NAT 的公网地址，能藏多少个主机？
> 关注端口号，如果给 TCP 端口号的 bit 是 16 bit，那么一个 NAT 就可以映射 2^16 个
* NAT 能“藏”多少主机，本质上是被它能用来区分不同连接的端口数限制的。
* 端到端的过程结束了
* 无连接到面向连接
> 端口号码的限制
* 需要 IP 以上的帮助
* NAT 无法支持端到端新协议
* P 2 P：bittorrent 强调端到端
![[image-898.png]]

* 加入一个桥，中间服务器，skype
* 接下来提高 IP 地址的长度
* TCP/IP 最开始没有什么安全性
* 后面 ssl，https 开始出现

## IPV 6
* 已提出就已经开始考虑安全性了
![[image-899.png]]

![[image-900.png]]


![[image-901.png]]
* **Source address and destination address:** each 16 bytes long.
* **Checksum**: removed entirely to reduce processing time at each hop
* Hop by hop：逐跳进行
![[image-902.png]]
* IPv 6 的新的数据结构
![[image-903.png]]
* "Next Header 只在**单个数据包内**串联头部"
* The routing header
> **The routing header** lists one or more routers that must be visited on the way to the destination.

![[image-904.png]]

### IPv6 notations
* 分成 8 个 4 位的 16 进制数
![[image-905.png]]
* 前面的 0 可以省略
* Second, one or more groups of 16 zero bits can be replaced by a pair of colons
* IPv4 addresses can be written as a pair of colons and an old dotted decimal number
* 大的数据包意味着冲突的增多
* 检测可以在传输层进行
* ICMP 协议：ping 命令，传输层协议
> 主机不可达，TTL 为 0
* **traceroute**：ICMP，但是 A 先发送 ICMP，TTL 设置为 1，做了一次传输之后，会返回 ICMP false，第一个路由器，那么第二个路由器把 TTL 设置为 2...
* ARP
> IP 地址找 Mac 地址，在链路层中，一个局域网内，得到 destination 的 mac 地址，所以 IP 层输出一个数据帧广播，包含想要发送的目的路由器的信息（IP 地址），然后获得这这个信息的所有路由器会检查这个信息是否是自己的，如果是，返回自己的 mac 信息
* 如果 ARP 跨路由器，跨局域网
> 把 mac 地址先设置为中转路由器的信息
* DHCP server
> 新的机器进来，host broadcasts “DHCP **discover**” 放入 mac 地址，DHCP server responds with “DHCP **offer**”，分配 ip 地址，这个 response 是广播的，host requests IP address 广播，DHCP server sends address: “DHCP **ack**”
* 网络中可能有多个 DHCP 服务器，第三步告诉所有 DHCP 选择的分配信息
* MPLS：
![[image-906.png]]

![[image-907.png]]

