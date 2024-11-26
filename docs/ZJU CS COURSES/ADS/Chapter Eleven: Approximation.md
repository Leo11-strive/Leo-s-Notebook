## Bin Packing  
**FirstFit** seudo code:  
```less  
for each item i  
    for each bin j  
        if i fits in j  
            place i in j  
            break  
    if i is not placed  
        create a new bin  
        place i in the new bin  
```  
![alt text](image-15.png)   
the ratio is 1.7  
**BestFit** seudo code:  
```less  
for each item i  
    find the bin j with the smallest room that i fits in  
    if i fits in j  
        place i in j  
    else  
        create a new bin  
        place i in the new bin  
```  
the ratio is approximately 1.7 too  
!!! note "Conclusion"  
  
    <center>Next fit is the worst, and first fit and best fit are approximately the same.</center>  

*they are all on-line algorithms*: place one by one, and the decision is made based on the current state.(落子无悔，之后无法改变)   

Not good!!!: ![alt text](image-16.png)  

### Off-line Algorithms  
view the entire item set before placing any item.  
Trouble maker: the large items  
  
        
**Solution: sort the items into non-increasing order, and then apply the first-fit algorithm.**  

![alt text](image-17.png)  


**the bound are updated**  
the ratio is about 1.2  
***Simple greedy heuristics can give ralatively good result***  







