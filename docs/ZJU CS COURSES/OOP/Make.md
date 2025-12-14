## How to write a Makefile  
* sum: objective + main.o+ sum.o  

* gcc main.o sum.o -o sum     

* main.o: main.c  sum.h   

    gcc -c main.c //-c only compile without linking 

sum.o: sum.c sum.h  

    gcc -c sum.c     
  
* make file only compiles necessary files.  
  
* a simpler version  
```makefile  
sum: main.o sum.o  
    gcc main.o sum.o -o sum  
main.o:sum.h  
sum.o=sum.h  
%.o:%.c  
    gcc -c $<  //universally compile all .c files
```  
* `$<` is the first prerequisite, $ is variable.   
```makefile  
OBJS=main.o sum.o  
sum: $(OBJS)  
    gcc $(OBJS) -o sum  
$(OBJS): sum.h  
%.o:%.c  
    gcc -c $<  
```   
```makefile  
OBJS=main.o sum.o  
sum: $(OBJS) 
    @echo "Linking $^ to $@" //@ not to show echo 
    gcc $^ -o $@// $@ is the target file, $^ is the prerequisites.
$(OBJS): sum.h
%.o:%.c    
 
    gcc -c $<  
```  
```makefile  
clean:  
    rm -f sum $(OBJS)  
```   
* `make clean` to clean the files.  
* `-Wall` to show all warnings.  
  
## fake targets  
* 'clean' is a fake target. 
* make will find the first target to compile  

```makefile
OBJS=main.o sum.o
all: sum
sum: $(OBJS)
    gcc $^ -o $@
$(OBJS): sum.h
%.o:%.c
    gcc -c $<
clean:
    rm -f sum $(OBJS)
```
  
* the '%.h' file can be omitted, but to make the '.h' program recompile when changed, it is necessary to add the '.h' file.
* run the program by `make` is also feasible.   
* `$ARGS$`: make ARGS="1 2 3" can do input    
* 通过宏改变程序中参数    
* '-MMD' in gcc, to generate the dependency file automatically, generate the '.d' file, with dependencies inside it  
* '-include DEPS=$(OBJS:.o=.d)' to include the '.d' file    






