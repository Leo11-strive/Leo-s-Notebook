## Containers  
* Also Known As: Collection   
* **STL**: Standard Template Library  
* Reduce development time and increase efficiency  
> Data-structures are already written and debugged    
  
  

## Why STL?
* Code readability  
* Robustness  
* Portable, maintainable, and easy  
  
## C++ Standard Library  
* a pair class:   
```cpp  
#include <utility>  
pair<int, int> p;  
p.first=1;  
p.second=2;  
```  
* Containers:  
    * vector  
    ``` cpp  
    #include <vector>  
    vector<int> v;  
    v.push_back(1);  
    v.push_back(2);  
    ```  
    * deque: double-ended queue 
    ``` cpp  
    #include <deque>  
    deque<int> d;  
    d.push_back(1);  
    d.push_front(2);  
    ```    
    * list: doubly linked list  
    ``` cpp  
    #include <list>  
    list<int> l;  
    l.push_back(1);  
    l.push_front(2);  
    ```  
    * set:  
    ``` cpp  
    #include <set>  
    set<int> s;  
    s.insert(1);  
    s.insert(2);  
    ```  
    * map: dictionary 
    ``` cpp  
    #include <map>  
    map<int, string> m;  
    m[1]="one";  
    m[2]="two";  
    ```    

  
    
* Basic Algorithms, sort, search  
* all identifiers are in the std namespace  

  
      
        
## Examples  
* vector  
``` cpp  
   
    #include <iostream>  
    #include <string>    
    #include <vector>//every type of container should include the corresponding header file
    using namespace std;  
    int main()
    {
        vector<int> v;  
        for(int i=0; i<10; i++)
        {
            v.push_back(i);//add elements to the end of the vector
        }  
        cout << v.size() << endl;  
        cout << v.capacity() << endl;  
        for(int i=0; i<v.size(); i++)
        {
            cout << v[i] << " ";    
            cout << v.at(i) << endl;
        }  
        vector<int>::iterator it;  
        for(it=v.begin(); it!=v.end(); it++)
        {
            cout << *it << " ";  
        }
    }  
```  

* Generic Classes:  
> `vector<int> v`; `vector<string> notes`  
> Have to specify two types: the type of the collection itself and the type of the elements that we plan to store in the collection  
  
* Constructors:    
   * `vector<int> v; vector<int> v1(v)`: two vectors, `v1` is a copy of `v`  
     
  
  
* Simple Methods:  
> `v.size()`: number of items
> `v.empty()`: whether the vector is empty    
> `==`, `!=`, `>`, `<`, `>=`, `<=`  
> `v.swap(v1)`: swap the contents of two vectors  
  
* iterators:  
> `vector<int>::iterator it`  
> `it=v.begin()`: iterator of the first position  
> `it=v.end()`: iterator of the last position    
  
* Element Access:  
> `v.at(index)`  
> `v[index]`  
> `v.front()`, `v.back()`    
  
* Add/Remove/Find:  
> `v.push_back(e)`  
> `v.pop_back()`  
> `v.insert(pos,e)`  
> `v.erase(pos)`  
> `v.clear()`  
> `v.find(first, last, item)`  
  
* Preallocate Memory:  
```cpp  

   vector<int> v(100); //capacity is 100 
   v[80]=1;    
   cout<<v[100]<<endl;//might not have a problem, sometimes the vector class add some extra capacity when preallocation 

```  
  
    
## List  
* same basic concepts as vector, but double linked list 
* `list<int> l;`  
* `l.push_back(e)`, `l.push_front(e)`  
* `l.pop_back()`, `l.pop_front()`  

  


  
