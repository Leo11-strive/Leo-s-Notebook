* 效率！
![[image-1101.png]]

![[image-1102.png]]

![[image-1103.png]]
* 命名，如何访问？通过路径

## File concept
* 连续的逻辑空间的数据
![[image-1104.png]]
* 使用者和创建者决定

![[image-1105.png]]
* 定长的好处就是方便定位
* 缺点就是碎片 overhead
* 变长就需要存储长度信息

## File Attributes
![[image-1106.png]]
* 会用 name（用户） 和 id（系统）
* 保存在目录结构里面
* 通过一张表，作为 matadata
* 目录本质上一个文件
* FCB

## File operations
![[image-1107.png]]
* Truncate：截取
* 目录也是一个文件
![[image-1108.png]]
* open file
> Open+read
![[image-1109.png]]
* perprocess table:
> Open system call, 打开和 read，进程的文件表，记录当前信息：当前文件信息和 access 的权限
* system wide table
> 存在指针指向文件，大小，创建时间，location，open count
* 为了防止数据不一致的问题
![[image-1111.png]]
* file lock
![[image-1112.png]]
> 强制锁和建议锁
![[image-1146.png]]

## Access Method
![[image-1113.png]]

![[image-1114.png]]

![[image-1115.png]]
* 可以使用 index file
![[image-1116.png]]

## Directory Structure
![[image-1117.png]]
* 一个目录中存在一堆FCB
* 文件名称和文件信息的对应（可以直接存指针，指向文件 inode）
* file system organization
![[image-1118.png]]
* 操作系统：卷：可以包含多块 disk
* partition：物理分层
![[image-1119.png]]

![[image-1120.png]]

![[image-1121.png]]

![[image-1122.png]]

* 加入 user 的信息

![[image-1123.png]]
* 文件名和文件信息在目录文件里
* 如何区分目录项还是文件
![[image-1124.png]]
* working directory
![[image-1125.png]]
* 共享
![[image-1126.png]]

![[image-1127.png]]
* 删除之后，就回发生悬空指针的问题
* 回指针：把其他所有指向这个文件的路径文件处理掉
> Cost too much
* inode：count 如果不是 1 的话，其中一个目录发出删除指令，可以返回要么错误，要么软指针
> Inode 里面存一个文件路径（字符串），那么再使用这个文件路径访问，kernel 就可以直接处理，没有指针悬空访问

![[image-1147.png]]
![[image-1148.png]]

![[image-1149.png]]

![[image-1150.png]]

## File Mounting
![[image-1151.png]]

![[image-1152.png]]

![[image-1153.png]]

![[image-1154.png]]

![[image-1155.png]]







