## Memory Layout  
![alt text](image.png)
The reason for using iteration to replace recursion:
- The size of the stack is limited
- The computer is vulnerable
- Registers and addresses are stored and protected in the stack
- Too much recursion leads to stack overflow

**Frame Pointer**:
Located at the bottom of the stack, for faster addressing

!!! tip
    <center>Use the "nickname"</center>  

![alt text](image-1.png)

W type operator:  
* Single word  

![alt text](image-2.png)  
!!! tip   
    <center>For all the jumping operations, `beq` and `bne` are enough</center>  

**`jal`, `jalr`**
!!! warning  
    <center>The MIPS instruction set is different from RISC-V, don't mix the two.</center>  

![alt text](image-3.png)  

**ASCII**: Abundant at first (English, Latin)  
**Unicode**: 32-bit character set (standard)  
* Widely used in programming languages   

## Byte/Halfword/Word Operations  
![alt text](image-4.png)  
!!! warning   
    <center>All the operations are presented in 32-bit in the experiment section.</center>  

![alt text](image-5.png)  
Reduce the usage of saved registers  
* In embedded development, every bit of memory matters  
* Although embedded development is difficult, it is the trend. Some students will be forced by life... (Quoted from Prof. Ma De) :joy:  

## Immediate number and address  
**If the number is too large, larger than 12-bit**  
`lui`:   
![alt text](image-6.png)  
contains storage of 20-bit  
??? Question "Why 20-bit?"  

    <center>when you want to add `addi`:computer:</center>  

![alt text](image-7.png)  

* SB-type *~~Not suitable for Chinese students~~ : 
指令地址一定要对齐(half-word alignment: preparation for ARM ISA) 
因此没有第一位
![alt text](image-8.png)    
* UJ FORMAT *:    
![alt text](image-9.png)  
When the address is too big, use `lui`  
## Collection of Instructions  
![alt text](image-10.png)  
## Synchronization in RISC-V   
* nobody can interrupt the atomic operation   
![alt text](image-11.png)
`lr.d` and `sc.d`   
* `lr.d` is used to load the value of the address into the register, thus the CPU remembers the operation lr.d   
* `sc.d` is used to store the value of the register into the address, lr.d and sr.d are a pair of operations, and the CPU will check if the value of the address has been changed during the operation. If it has not been changed, the operation is successful. Ohterwise its not.  
!!! example  

    r20->7
    then lr.d s2 (r20)  
    s2=7  
    CPU remebers the procedure   
    sc.d rd (r20) s2  
    the value on the (r20) address might be changed because of multi-core structure, when encountered this situation, 1 will be stored in rd(unsuccessful, the value of s2 cannot be stored in that memory).  
![alt text](image-12.png)
***No interruption is allowed***  
in cpu, one paticular address need to be locked, so that no other cpu can access it.  
![alt text](image-13.png)  

## Translating and staring a program  
![alt text](image-14.png)  
![alt text](image-15.png)  
![alt text](image-16.png)   
when learning compiling      
**.dll** is a dynamic link library, which is a collection of small programs, any of which can be called when needed by a larger program that is running in the computer. The small program that lets the larger program communicate with a specific device such as a printer or scanner is often packaged as a DLL program (usually referred to as a DLL file).  
![alt text](image-17.png)  
*Starting Java Applications*  
the virtual machine of Java allows Java to be run on any platform.  
![alt text](image-18.png)  
## A C Sort Example To Pull It All Together  
![alt text](image-19.png)  
![alt text](image-20.png)  
## Arrays Versus Pointers  
![alt text](image-21.png)(comilers are smart enough to optimize the code, to change the array to pointer and vice versa)  
![alt text](image-22.png)  
## Real Stuff: MIPS Instrcuctions  
all 32-bit instructions  
![alt text](image-23.png)
the difference:  
![alt text](image-36.png)  
reverse version of RISC-V  
the store instruction is the same as the load instruction  
not successful in the market:joy:  
## Real Stuff: The Intel x86 ISA  
8080: 8-bit  
8086: 16-bit  
**Further evolution**:  
* AMD64(64)  
regs have 32bits and 64bits  
Too complex!!!  
Mac: ARM, 外星科技！！！  
## Implementing IA-32  
* All things taught in this course is RV64I  
There is extentions: M,A,F,D,C    
## Fallacies  
* Poerful instruction -> higher performance No!!!  
* Backward compatibility -> ISA doesn't change No!!!  







    