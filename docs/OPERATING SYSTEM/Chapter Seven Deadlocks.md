* 如何描述死锁
* 关于解决死锁的方法
![[image-739.png]]
* 各种各样的死锁

* A set of blocked processes each holding a resource and waiting to acquire a resource held by another process in the set.

## Deadlock Characterization
![[image-740.png]]
* 互斥条件：只有一个进程可以访问某个资源
* hold and wait: a process holding at least one resource is waiting to acquire additional resources held by other processes.
* a resource can be released only voluntarily by the process holding it, after that process has completed its task.
![[image-741.png]]
* 循环等待
![[image-742.png]]
* instances: 
> 一种资源的多个实例（多核 CPU，多个打印机）
![[image-743.png]]
* 一个有向图

## Resource Allocation Graph
![[image-744.png]]
* 死锁一定环路，环路不一定有死锁
![[image-745.png]]

![[image-746.png]]

## Deadlock Prevention
![[image-747.png]]

![[image-748.png]]
* 运行的时候申请，没有 hold 的资源
* 分配资源的时候直接把它需要的全给他
* 但是可能会有饥饿
![[image-749.png]]
* 大量的处理，大量的等待
![[image-750.png]]
* 进程请求资源的时候，先请求 number 比较低的资源（intuition），这样的话，破坏循环
> 不会出现，因为不会回环（先申请高再申请低）
* 资源使用效率低，但是没有上一个那么低

* 需要知道一些 prior information
![[image-751.png]]

![[image-752.png]]

![[image-753.png]]
* 安全序列是根据当前系统 resource 的
* 其实只要代码排布，动态资源变化（外界），都可能让 safe state 运行完毕

* unsafe 除了死锁，还有资源不够

## Resource Allocation Graph Algorithm
* _Claim edge_ _P__i_ → _R__j_ indicated that process _P__i_ may request resource _R__j_; represented by a dashed line.
* Request edge converted to an assignment edge when the  resource is allocated to the process.
* When a resource is released by a process, assignment edge reconverts to a claim edge.
* The request can be granted only if converting the request edge to an assignment edge does not result in the formation of a cycle in the resource allocation graph
* 资源分配之后是否存在环

## Banker's Algorithm
* 资源分配好了之后，是否存在一个安全序列
![[image-767.png]]
### Data Structures
![[image-768.png]]

![[image-769.png]]
* 把这个 i 作为第一个，第二个，第三个排列

![[image-770.png]]

![[image-771.png]]
* 检测是否有环
* Periodically invoke an algorithm that searches for a cycle in the graph. If there is a cycle, there exists a deadlock.
* An algorithm to detect a cycle in a graph requires an order of _n_2 operations, where _n_ is the number of vertices in the graph.
* 不能放有任何死锁的可能性

![[image-772.png]]
![[image-773.png]]

![[image-774.png]]

![[image-775.png]]

* Selecting a victim – minimize cost.
> 有可能出现 starvation
* Rollback – return to some safe state, restart process for that state.