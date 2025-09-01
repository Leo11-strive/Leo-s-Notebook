## Class Four   



* destructor   

```cpp  
struct point  
{ 
    int x, y;  
    point(int x, int y) : x(x), y(y) {}  
    ~point() { cout << "destructor" << endl; }  
}  
```     




* the call of the destructor is automatic, thus the latter object created will be destroyed first.    

* living life span of a local object: from the beginning of the block to the end of the block.   

* `goto`: jump to the label, which is a bad practice.   


* **aggregate initialization with Ctor**  
* use {} to initialize the object.   

```cpp  
point p1 = {1, 2};  
```  
if there is a initialization funtion  
we should use the initialization function to initialize the object.   

```cpp  
point p1[]={point(1, 2), point(3, 4)};  
```  
  
## inside Object   

* A program is a bounch of objects telling each other what to do by sending messages.----Alan Kay  
  
* **access control**      

> * public: can be accessed by any function.    

> * private: can only be accessed by the member functions of the class.  
>     * b.j is illegal if b is an object of class B and j is a private member of class B.
> * if no access control is specified, the default is public (struct) 
        
!!! tip   

    <center>Different objects of the same class, their private members can be accessed by each other.</center>  
      
* Friend:      

> * a friend function can access the private members of the class.  
```cpp  
class A  
{  
    friend void f();  
};  
```  
  
## Class    


* the difference between class and struct is the default access control.  
* the default access control of class is private.  
* the default access control of struct is public.    
  
## Where are the objects?   

* Fields, parameter, local variables  
* global variable  
  
## Static Initialization Dependency   

* Order between files is unspecified  
* the tendency is not to use any global variables.  

## Static    

* two basic meanings:  
>   static storage  
>   restricted access    
  
### the use of "static" in C++   

* static applied to objects   

> create when called, destroyed when the program ends.  

* Can we apply static to members?   

> static member: shared by all objects of the class, thus not belongs to any object  
> therefore, a definition of the static member is necessary. `int StatMem::m_h;`  
> static funtion: 














