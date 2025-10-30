* when some code in the process need to wait
* set time limit (what if we lower the time bound?)
> Faster response
* what if we check every time to switch between different task in a single process? (Whether there is data)
> Lots of check, not efficient
![[image-256.png]]

## Thread Cancellation
![[image-271.png]]

## Signal Handling
![[image-272.png]]

## Thread Pooling

![[image-273.png]]
* 提前创建线程
* 方便 many to many 调度

## Thread Specific Data
![[image-274.png]]
* local 信息

## Scheduler Activation
* Both M: M and Two-level models require communication to maintain the appropriate number of kernel threads allocated to the application
![[image-275.png]]
