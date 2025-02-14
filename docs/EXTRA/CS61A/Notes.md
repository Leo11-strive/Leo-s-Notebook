## Lecture 3: Control  
* **doctest**: a way to test your code by writing examples in the docstring of the function.  
```python  

def square(x):  
    """Return the square of x.  
    >>> square(3)  
    9  
    >>> square(2)  
    4  
    """  
    return x * x  
```   
```bash  

python3 -m doctest -v file.py  
```    
  
* **default arguments**:  
```python  

def f(x, y=3):  
    return x + y  
```  
  
> indicates that if the argument y is not transmitted, it will be 3.  
  
  
* **change text to interactive codes**:  
``` bash   
 
    python3 -i file.py  
```  

