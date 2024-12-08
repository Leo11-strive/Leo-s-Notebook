## Structure of a Verilog Program  
* A Verilog program is structured as a set of modules, which may represent anything from a collection of logic gates to a complete system. Modules are similar to classes in C++, although not nearly as powerful. A module specifies its input and output ports, which describe the incoming and outgoing connections of a module. A module may also declare additional variables. The body of a module consists of:  
> * `initial` constructs, which can initialize reg variables  
> * Continuous assignments, which define only combinational logic  
> * always constructs, which can define either sequential or combinational logic  
> * Instances of other modules, which are used to implement the module being defined  
## Representing Complex Combinational Logic in Verilog   
### Continuous Assignments 
```verilog  
module half_adder(A,B,Sum,Carry);  
    input A,B;   
    output Sum,Carry;    
    assign Sum = A ^ B;  
    assign Carry = A & B;  
endmodule  
```    

* A continuous assignment, which is indicated with the keyword assign, acts like a combinational logic function: the output is continuously assigned the value, and a change in the input values is reflected immediately in the output value.      
!!! warning  

    <center>Assign statements are one sure way to write Verilog that generates combinational logic. For more complex structures, however, assign statements may be awkward or 
    tedious to use. It is also possible to use the always block of a module to describe a combinational logic element, although care must be taken. Using an always block allows the inclusion of Verilog control constructs, such as if-then-else, case statements, for statements, and repeat statements, to be used. These statements are similar to those in C with small changes.</center>  

### Using always Blocks for Combinational Logic  
!!! note   

    <center>sensitivity list The list of signals that specifies when an always block should be re-evaluated.</center>  

```verilog  
always @(list of signals that cause reevaluation) begin
 Verilog statements including assignments and other
control statements   
end  
```   

* When an always block is specifying combinational logic, the sensitivity list should include all the input signals. If there are multiple Verilog statements to  be executed in an always block, they are surrounded by the keywords begin and  end, which take the place of the { and } in C   
* `=`: blocking assignment, which means that the right-hand side is evaluated and assigned to the left-hand side before the next statement is executed.  
* `<=`: nonblocking assignment, all right-hand sides of the assignments in an always group are evaluated and the assignments are done simultaneously  
=== "Mux4to1"  
    ```verilog  
    module mux4to1(In1,In2, In3, In4, Sel, Out);  
        input [63:0] In1, In2, In3, In4;  
        input [1:0] Sel;  
        output reg [63:0] Out;  
        always @(In1 , In2 , In3 , In4 , Sel) begin  
            case (Sel)  
                2'b00: Out <= In1;  
                2'b01: Out <= In2;  
                2'b10: Out <= In3;  
                2'b11: Out <= In4;  
            endcase  
        end
    endmodule  
    ```    
=== "ALU"    
    ``` verilog  
    module ALU(ALUctl, A,B,ALUOut, Zero);
       input [3:0] ALUctl;  
       input [63:0] A,B;  
       output reg [63:0] ALUOut;  
       output Zero;  
       assign Zero = (ALUOut == 64'b0);  
       always @(A,B,ALUctl) begin  
           case (ALUctl)  
               4'b0000: ALUOut <= A + B;  
               4'b0001: ALUOut <= A - B;  
               4'b0010: ALUOut <= A & B;  
               4'b0011: ALUOut <= A | B;  
               4'b0100: ALUOut <= A ^ B;  
               4'b0101: ALUOut <= ~A;  
               4'b0110: ALUOut <= ~B;  
               4'b0111: ALUOut <= A << 1;  
               4'b1000: ALUOut <= A >> 1;  
               4'b1001: ALUOut <= A;  
               4'b1010: ALUOut <= B;  
               4'b1011: ALUOut <= 64'b0;  
               4'b1100: ALUOut <= 64'b1;  
               4'b1101: ALUOut <= 64'b0;  
               4'b1110: ALUOut <= 64'b1;  
               4'b1111: ALUOut <= 64'b0;  
           endcase  
       end
    endmodule  
    ```     
------------------- 
  

=== "Beware" 
    !!! warning "Beware"  

        <center>Since only reg variables may be assigned inside always blocks, when we want to describe combinational logic using an always block, care must be taken to ensure that the reg does not synthesize into a register</center>    
=== "Tips"  
    !!! tip "Tips"  

        * Place all combinational logic in a continuous assignment or an always block.  
        * Make sure that all the signals used as inputs appear in the sensitivity list of an always block.  
        * Ensure that every path through an always block assigns a value to the exact same set of bits








   