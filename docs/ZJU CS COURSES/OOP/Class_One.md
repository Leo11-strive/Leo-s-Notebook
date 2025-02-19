## Intro    
* Buzzword  
> encapsulation  封装
> inheritance 继承  
> polymorphism 多态  
> overriding 覆盖  
> interface 接口  
> cohesion 内聚  
> coupling 耦合  
> collection classes 容器类  
> template 模板  
> responsibility-driven design 责任驱动设计   
   
* Book reference: Thinking in C++    

* The C language  
> Efficient programs  
> direct access to machine  
> flexible   
  
  
* C++ improvements  
> data abstraction  
> access control  
> initialization & cleanup  
> function overloading  
> stream for I/O    

  



* The first program   
``` c++  

    #include <iostream>  
    using namespace std;  
    int main()  
    {
        cout << "Hello, World!"<< endl;  

    }  

```    
* iostream is a header file name without a .h suffix. iostream.h happens to exist, yet it is an outdated version  
* `<<`: inserter  
* `cin >> variable`: extractor    
* use `;` to connnect the shell commands  


## Format output  
* `#include <iomanip>`: manipulators are special functions that can be included in the I/O statement to alter the format parameters of a stream.  
* `endl` is one of them  
* `setw(int width)`: set the output width  
* `setprecision(int n)`: set the number of digits in decimal parts 
  
## String  
* `#include <string>`: c don't have a string type   
* `string str`: a string  
* `str="Hello"` , `str("Hello")`, `s3{"see see"}` for every type, yet when it comes to the third way of initialization, one should add `g++ -std=c++11` when compiling.  
* objects can transfer contents through `=`  
* `cin`: only read the first word  
* `getline(cin, str)`: read the whole line    
* `string s[0]`: the first character of the string, the same as the array in c    
* `string s2=string s1+ string s3`: string concatenation  
* `s2.length()`: the length of the string  
* `s2(s3,2,3)`: the third to the fifth character of s3, `2` is the starting position, `3` is the length of the substring    
* `s2.substr(2,3)`: the same as above    
* `insert(size_t pos, const string& str)`: insert a string into another string  
* `erase(size_t pos, size_t len)`: erase a substring  
* `replace(size_t pos, size_t len, const string& str)`: replace a substring with another string  
* `find(const string& str, size_t pos=0)`: find the first occurrence of a substring  
* `append(const string& str)`: append a string to another string     
* `string *p=&s`: a pointer to a string  
* `s->length()`: the length of the string, the same as `(*s).length()`  
* `string s1, s2; s1=s2;`: copy the string, two different objects  
* `string *p=&s1; string *q=&s2; p=q;`: two pointers point to the same object `s2`  














