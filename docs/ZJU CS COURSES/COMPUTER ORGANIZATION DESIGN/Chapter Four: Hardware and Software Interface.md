## Computer Organization  
![alt text](image-37.png)  
The main content is datapath  
## Outline  
* Intro  
* Logic Design Convention  
* Building a datapath  
* a simple implementation scheme  
## 4.1 Introduction  
* CPU performance factors:    
  * Instruction count  
    * determined by ISA and compiler  

   * CPI and clock cycle time  
    * determined by CPU hardware  


### Instruction Execution Overview  
* First two steps are identical for all instructions:  
> Fetch instruction from the memory   
> Decode instruction and read the  operands from the register file   
* Next step is deterined by instruction class  
> Memory reference, Arithmetic-logical, branches  
* Depending on instruction class  
> ALU calculation(memory address load/store), result generate, different control(branches)  
> access memory  
> have a control over PC  
**Overview**  
![alt text](image-65.png)  
## 4.2 Logic Design Convention  
* Information are encoded in binary to make the information conveyed less sensitive, hence more resilient to noise  
> One wire per bit, multi-bit encoded on multi-wire buses  
> combinational curcuits perform data processing  
> sequential circuits store state  
**Sequential Elements**  
* register: stores data in circuit  
> when clk changes from 0 to one, update with delay  
> a write control shall be implemented in regs->Update only when write control is `1`  
**Clocking Methodology**  
* Combinational logic transforms data during clock cycles  
> its longest delay determines the clock period   
## 4.3 Building a Datapath  
* Datapath:  
> elements that process data and addresses in the CPU  
**basic procedure**  
![alt text](image-66.png)  














































































































































































































































   * Fetch operands 
   * Execute operation  
   * Store result  

* ALU usage:  
sd, and ld need ALU to calculate the address  
beq need ALU to compare the two operands  
* An overview :  
![alt text](image-39.png)  
the upper two instructions are jumping instructions  
## Control 
![alt text](image-40.png)  
## 4.2 Logic Design Convention  
![alt text](image-41.png)  
Basic combinational logic  
**Sequential Elements**  
![alt text](image-42.png)  
everything is triggerd by the clock    
the write signal in control: 使能信号  
![alt text](image-43.png)  
时钟周期一定是按照组合逻辑处理速度来决定的 
## 4.3 Building a Datapath  
* Datapath  
![alt text](image-44.png)  
![alt text](image-45.png)  
### R-type (0110011)  
`lr.d`: 做锁，无需实现  
### I-type (0000011，0010011，1100111)   
### S-type (0100011)  
### SB-type (1100011)  
### U-type (0110111)  
### UJ-type (1101111)  
### Instruction execution in RISC-V 
![alt text](image-46.png)  
every instruction shall be dealed in this process  
#### Instruction Fetch  
* PC: Program Counter: PC+4  
**R-Format Instruction**:  
![alt text](image-47.png)  
**Load/Store Instruction**:  
![alt text](image-48.png)  
ImmGen: 立即数生成单元  
**Branch Instruction**:  
![alt text](image-49.png)  
remember that the immediate number shall be multiplied by 2 before added back to PC    
**Composing the element**  
![alt text](image-50.png)  
Control 单元有两个  
**I type**  
![alt text](image-52.png)  
**S type** (`beq`) 
![alt text](image-53.png)   
**SB type**  
![alt text](image-54.png)  
!!! tip  

    <center>Bear in mind that immi shall shift left 1 bit</center>  

**Jal/J type**  
![alt text](image-55.png)  
**Full datapath**    
![alt text](image-56.png)  
## 4.4 A Simple Implementation Scheme  
* Control: follow datapath usage  
![alt text](image-57.png)  
* ALU:  
![alt text](image-58.png)  
!!! note  

    <center>ISA determines the complexity of the circuit</center>  

![alt text](image-59.png)  
除了ALU，其他都可以通过opcode来判断  
![alt text](image-60.png)  
X means the signal is not used, not cared  
![alt text](image-61.png)  
二级解码：  
![alt text](image-62.png)  
![alt text](image-63.png)   
**Datapath with control**  
![alt text](image-64.png)  


