## Intro  
**Approximation Algorithms**  
Find near optimal solutions in polynomial time   
**Approximation Ratio**  
$\rho (n) = max(\frac{C(A)}{C(OPT),\frac{C(OPT)}{C(A)}}$  
If an algorithm achieves an approximation ratio of $\rho (n)$, we call it a $\rho (n)$-approximation algorithm.  
**Approximaion Scheme**  
A polynomial-time algorithm that, for any $\epsilon > 0$, produces a solution whose cost is within a factor of $1 + \epsilon$ of the optimal cost.  
**PTAS**: Polynomial-Time Approximation Scheme   
**FPTAS**: Fully Polynomial-Time Approximation Scheme  
!!! note  "The difference between PTAS and FPTAS"  
    <center>PTAS: the running time is polynomial in the size of the input$ , for example $O(n^{1/\epsilon})$</center> 
    <center>FPTAS: the running time is polynomial in both the size of the input and $1/\epsilon$,for example $O((1 \frac \epsilon)^2 n^3)$</center>  




    




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

## The Knapsack Problem  
A knapsack with a capacity M is to be packed. Given N items.  Each item  i  has a weight  wi  and a profit  pi .  If  xi is the percentage of the item i  being packed,  then the packed profit will be  pi xi .  The goal is to maximize the total profit while keeping the total weight less than or equal to M.  
**0-1 version**: each item can be packed at most once.  
??? Question "What if we are greedy on taking the maximum profit or profit density?"  

    <center>The approximation ratio is 2.</center>  
    Proof:  
    
## The K-center Problem  
**K-center problem**: Given a set of points in a metric space, the goal is to find a set of K centers such that the maximum distance from any point to its nearest center is minimized.  
**What is a distance?**  
identity,symmetry and triangle inequality  






