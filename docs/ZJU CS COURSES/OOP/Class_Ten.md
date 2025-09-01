
* vptr: 在构造函数中赋值，其他情况下无法赋值



* copy时， class隐含一个copy构造函数，这个copy构造函数是编译器自动生成的，但是如果我们自己定义了一个copy构造函数，那么编译器就不会再生成一个了

* 如果函数是在 return value 中创建，那么从 local copy 至临时return value 这一段 copy 就可以被编译器省略，如：
```c++
Person copy_

func( char *who ) {

Person local( who );

local.print();

return local; // copy ctor called!

}

Person nocopy_

func( char *who ) {

return Person( who );

} // no copy needed!
```

* **Construction vs assignment**:
>  every object construct once
>  be destroyed once
>  once constructed, can be assigned multiple times

> [!abstract] tips
> * Pass in an object if you want to store it,
> * Pass in a const pointer or reference if you
> * want to get the values
> * Pass in a pointer or reference if you
> * want to do something to it
> * Pass out an object if you create it in the function
> * Pass out pointer or reference of the passed in only
> * Never new something and return the pointer

* **移动构造函数**
>  Moveable c(move(a)); 这样的语句。这里的 a 本来是一个左值变量，通过std:: move 将其转换为右值。这样一来， a.i 就c 的移动被构造函数设置为指针空值。由于 a 的生命期实际要到所在的函数结束才结束，那么随后对表达式 `*a.i` 进行计算的时候，就会发生严重的运行时错误


* 