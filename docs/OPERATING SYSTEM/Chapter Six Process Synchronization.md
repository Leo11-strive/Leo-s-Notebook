* Concurrent access to shared data may result in data inconsistency
* Maintaining data consistency requires mechanisms to ensure the orderly execution of cooperating processes
> 进程之间的数据依赖
* Cooperating process can affect or be affected by the execution of another process
![[image-409.png]]
* 可以加 count，但是 count 的问题是也要注意这两个调用的 consistency
![[image-410.png]]
![[image-411.png]]

![[image-412.png]]
* $S3$ 之后又出现了 producer 的写入请求，如何处理呢？
* A race condition is a situation in which a memory location is accessed concurrently, and at least one access is a write.
![[image-413.png]]
* 还是会产生