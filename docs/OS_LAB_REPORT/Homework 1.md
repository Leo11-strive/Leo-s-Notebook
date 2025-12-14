![[image-777.png]]
```c
number x， y(信号量)//计算箱子中的A，B数量
//worker1
do{
加工A
P(x)
放入箱子中
x++
V(x)
}while（1）
//worker2
do{
加工零件B；
P(y)
把零件B放入箱中
y++
V(Y)
}
while(1)
//worker3
do{
while(x<1&&y<2);
P(x,y)
x--;
y--;
y--;
X(x,y)
}
```


![[image-778.png]]
```c
//写者优先
wread读锁
wr写信号量
queue写队列

Readers
do{
P(wr)
if(wr!=0)
V(wr)
else
P(wread)
read
V(wread)
V(wr)
}
Writers
{
  P(wr)
  
  if(wr==0)
    wr++
    V(wr)
    P(wread)
       //开始写
       if（wr！=0）
          
          开启queue排第一个的写者
          
       if（wr==0）
          V(wr)
    V(wread)
   else(Wr!=0)
     wr++
     加入queue
     V(wr)

}

//读写公平
w：读写
queue
read
P(w)
 if(w==0)
   w++
   V(w)
   read
   P(w)
   w--
   
   if(w!=0)
      
      开启queue中的第一个
      V(W)
   else
     V(W)
      
  else(w!=0)
    P(W)
    w++
    V(w)
    queue++
    
    
write
P(w)
 if(w==0)
   w++
   V(w)
   write
   P(w)
   w--
   
   if(w!=0)
      
      开启queue中的第一个
      V(W)
   else
     V(W)
      
  else(w!=0)
    P(W)
    w++
    V(w)
    queue++
  



```