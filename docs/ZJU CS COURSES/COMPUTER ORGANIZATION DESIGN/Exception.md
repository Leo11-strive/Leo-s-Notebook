## CPU within Exception  
* execute a new function when an exception occurs  
* CPU don't need to save time for unexpected events  
* `bne`, `beq` will cause a diffferent operation in PC  
* but sometimes the hardware will urge the CPU to do something unexpected  
* within the processor: Expection  
* from outside the processor: Interrupt  
* **What must the processor do**: 
> A predefined process routine  
## RISC-V Privileged  
* CPU: user operation, many regs cannot be accessed, only access to ports provided by operating system  
* Monitoring: Operating System (S) 
* Machine Mode: highest privilege, having access to everything  
## Control and Status Registers  
* Environment call(ecall) MEPC->&ecall(exception, interrupt is PC+4)
* Breakpoint (ebreak)  MEPC->&ebreak  
  




    



