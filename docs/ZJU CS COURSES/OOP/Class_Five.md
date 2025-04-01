
# Class Five

* when entering a function, the compiler will pre-allocate spaces for variables in the function.

## Reference

`char &r=c`
> r is a reference to c, which means r is an alias of c. r and c are the same variable.

* as a function parameter

```cpp
void swap(int &a, int &b){
    int temp=a;
    a=b;
    b=temp;
}
```

* left and right

> reference only accepts left value, not right value.
> reference there is no NULL
> a pointer that cannot calculate
> no pointers to reference
> but reference of a pointer `int * &p`

* left: variables, reference, result of operators `*, [], ., ->`

* 右值引用: `int &&r=5`, prolong the lifespan of a temporary variable, 提高运算速度，减少了一次析构和构造的时间

## Constant

* **const**
* constants are variables that cannot be modified
* const 后面的东西不能被修改（星号左右）
* `const point p`: this pointer is a const
* constant field in class

> has to be initialized in the constructor list
> cannot be used as the size of an array in the class

## dynamic allocated memory

* new
* delete
