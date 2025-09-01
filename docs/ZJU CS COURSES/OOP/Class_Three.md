## Preclass Quiz  
* after deleting ls.erase(it), 'it' will return the next element  
* `vector<int>::iterator it=ls.end();`: returns an iterator pointing to the past-the-end element in the list container.  
  
## Defining Class  
* Point  
> Pointer transmit is more efficient than passing the whole structure.    
  
* Why all these functions need to pass the point structure pointer?  
> to make the function be more universally used, that is , dealing with all kinds of point structures.  
  
* Migrate to C++  
> move the function to the class, and make it a member function.  
'''cpp  

struct Point  
{  
    int x;
    int y;
    void setX(int x);  
    void setY(int y);  
    int getX();  
    int getY();  
    void print();
};    
void Point::setX(int x)  
{  
    this->x=x;  
}  

'''    
'''cpp   
void S::f(){  
    ::f(); //call the global function f 
    ::a++; //access the global variable a 
}
'''    
* the fetching critiria of the member function    
> **this** pointer: a pointer to the object that the member function is called on.  
> **p.prt(); //Point::prt(&p);**  
> the local variable will override the outer variable 
  
## Object  
* **Object**: an instance of a class , attributes + service , data+operation   
> encapsulation: data hiding  
*  object oriented programming:  
> models designed can be used in multi-circumstances.  
> rather, procedural programming is more like a one-time use.  
  
* **UML**: Unified Modeling Language  
* class is mostly the same with struct in cpp, but mostly...  

* Every object has a type  

* **.h**: header file, to be portable, the body of the class should be in the .cpp file, which are hidden from the user.  **Bear in mind the basic pinciple of encapsulation**  
  
## Declaration and Definition  
* a .cpp file is a compile unit  
* **Declaration**:  
> tells the compiler that the class exists, but not the details.  
* **Definition**:  
> tells the compiler the details of the class.  
  
* the difference between <> and "" in the include statement:  
> <>: search the system path, INCLUDE(environment variable), -I,  
> "": search the current path, prior to the system path, then being similar to the <>  
> open source libraries: put the opensource library in a particular path, and add the path to the INCLUDE environment variable.  
  
* Standard header files:    
'''cpp
#ifndef _POINT_H  
#define _POINT_H
#endif  
'''  
  
* What if not initializing the object?  
> no specific value  
  
* **init** : not safe enough  
* constructor:  
> **Point()**: default constructor(without a parameter) , no return 
'''cpp  

   class Point  
   {  
       int x;  
       int y;
       Point(int x=0, int y=0);
   };  
   Point p(1,2);      

'''        
* initializer list  
> **Point(int xx=0, int yy=0):x(xx),y(yy){}**   
> the last is the inner constructer function to do initialization.  

 















