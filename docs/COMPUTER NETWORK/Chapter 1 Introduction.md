## Uses of Computer Network
* Computer Network: a collection of. Interconnected, autonomous computing devices (自治设备集合)
* Distributed system（互联，加入中间建，让用户用起来好像是一个整体，a more high level) WWW: 不需要关心服务器 ip，就好像是一台机器
### Access to Information
* client-server model:
> [!note]
>  ![[image-20.png]]
* involves requests and reply
* peer to peer model:
> No fixed clients and servers (bit torrent)
> ![[image-21.png]]
### Person-to-Person Conmmunication
![[image-22.png]]

### Electronic Commerse
![[image-23.png]]
### Entertainment and IoT
* Ubiquitous Computer
* Internet of things
* RFID: 标签无源，阅读器有源，两者沟通受限
![[image-24.png]]

* multihop topology
![[image-25.png]]

* Broadband Access Network: 有线网络
* Mobile and Wireless Access Network
* > related yet not identical
* Content Provider Networks: Content delivery network (CDN) deliver content to users around the world
* Cloudfare: free
* Transit Networks: 转接网络：链接不同的网络
* Enterprise Network：ZJUWLAN

## Network Tech， Local to Global
> [!note] Classification
> ![[image-26.png]]

> [!info] Scale
> ![[image-27.png]]

* 个人局域网：蓝牙，RFID
* 局域网：无限（WIFI），有线（以太）
* > size is restricted, worst case transmission bounded, high speed
![[image-28.png]]

* How the channel is allocated? (from the perspecive of MAC) 介质访问控制
![[image-29.png]]
* Power-line networks can also let devices that plug into outlets broadcast information throughout the house.
* Metropolitan area work
* WAN: 
![[image-30.png]]
> [!note] 
> ![[image-31.png]]

* WAN vs LAN: In WAN, host and subnet are owned and operated by different people. Routers will usually connect different kinds of networking technologies. Not only computers can be connected to subnet, but also entire LANs.
> [!info] Different WANs
> ![[image-32.png]]
![[image-34.png]]

> [!note]
> ![[image-35.png]]

* 4 G only support packet switch
* Handover:
![[image-36.png]]
* Cellular design: The coverage area is divided into cells
* Within a cell, users are assigned channels that do not interfere with each other, and other adjacent cells
* WIFI: Wireless networking with a base station. Ad hoc networking.
* > Multi-path fading
* **CSMA** deals with multiple transmissions over the broadcast channel. Faces problems when there are **hidden terminals**


## Network Protocols
* A **protocol** is an agreement between the communicating parties on how communication is to proceed.
* Service interfaces:
* > The interface defines which primitive operations and services the lower layer makes available to the upper one.
* > It is common that different hosts use different implementations.
* A list of protocols used by a certain system, one protocol per layer, is called **a protocol stack**.
![[image-37.png]]

* Network Protocols: Connections and Reliability
![[image-38.png]]


![[image-39.png]]

* Packets sent in a simple client-server interaction on a connection-oriented network.
![[image-40.png]]
* **A** **service** is a set of primitives (operations) that a layer provides to the layer above it.
* **A** **protocol** is a set of rules governing the format and meaning of the packets, or messages that are exchanged by the peer entities within a layer.
## Reference Models
### OSI

* Reference models: The OSI reference model (Open Systems Interconnection)
![[image-41.png]]
* Physical layer:  Concerned with transmitting raw bits over a communication channel.
* Data link layer: this layer is to take a raw transmission facility and transform it into a line that appears free of undetected transmission errors to the network layer.
![[image-42.png]]
* Network layer: This layer is concerned with controlling the operation of the subnet.
* Transport layer: To accept data from the session layer, split it up info smaller units if need be, pass these to the network layer, and ensure that the pieces all arrive correctly at the other end.
* Session layer: To allow users on different machines to establish sessions between them.
* Presentation layer: The presentation layer is concerned with the syntax and semantics of the information transmitted.
* Application layer: This layer contains a variety of protocols that are commonly needed.

### TCP/IP
![[image-43.png]]
* Link layer:
![[image-44.png]]

* Internet layer:
![[image-45.png]]
![[image-46.png]]

* Application Layer
![[image-47.png]]
* Comparison
![[image-48.png]]

* Metric Units:
![[image-49.png]]


