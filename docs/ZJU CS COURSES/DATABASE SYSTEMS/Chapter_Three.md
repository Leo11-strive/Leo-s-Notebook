# Introduction to SQL  
  
* **SQL** : define the structure of the data, modify data in the database, and specify security constraints.
 
--------------------------------  

## Overview of the SQL Query Language  
  
* **Data-definition language (DDL)**:The SQL DDL provides commands for defining
relation schemas, deleting relations, and modifying relation schemas.  
* **Data-manipulation language (DML)**: The SQL DML provides the ability to query
information from the database and to insert tuples into, delete tuples from, and
modify tuples in the database.  
* **Integrity**  
* **View Definition**  
* **Transaction Control**  
* **Embedded SQL and Dynamic SQL**   

* **Authorization**  
   
---------------------------------   

## 3.2 SQL Data Definition  
### 3.2.1 Basic Types   

* **CHAR(n)**: A fixed-length string of n characters. The full form, CHARACTER(n), is also supported.  
* **VARCHAR(n)**: A variable-length string with a maximum size of n characters. The full form, CHARACTER VARYING(n), is also supported.  
* **INT**: A 4-byte integer. The full form, integer, is also supported.  
* **SMALLINT**: A small integer.     
  
* **NUMERIC(p,d)**: A fixed-point number with p digits(plus a sign), d of which are to the right of the decimal point.   
  
* **REAL, DOUBLE PRECISION**: Floating-point numbers.  
  
* **FLOAT(n)**: A floating-point number with at least n bits of precision.  
  
* **NULL**: The null value.    


=== "varchar and char"
    !!! note  

        <center>The char data type stores fixed-length strings. Consider, for example, an attribute A of type char(10). If we stored a string “Avi” in this attribute, seven spaces are appended to the string to make it 10 characters long. In contrast, if attribute B were of type varchar(10), and we stored “Avi” in attribute B, no spaces would be added. When comparing two values of type char, if they are of diﬀerent lengths, extra spaces are automatically attached to the shorter one to make them the same size before comparison.<center>  

=== "usage"   
    !!! note   
      
        <center>As a result, even if the same value “Avi” is stored in the attributes A and B above, a comparison A=B may return false. It is recommended to always use the varchar type instead of the char type to avoid these problems.<center>  

    


       
!!! tip  
   
    <center>SQL also provides the nvarchar type to store multilingual data using the Unicode representation. However, many databases allow Unicode (in the UTF-8 representation) to be stored even in varchar types.<center>  
      
        
### 3.2.2 Basic Schema Definition  

* **create table**: We define an SQL relation by using the create table command.  
  
!!! tip "code"  

    ```sql  
    create table department  
       (  
        dept_name varchar(20),  
        building varchar(15),  
        budget numeric(12,2),  
        primary key (dept_name)
       );  
    ```   
  
* **integrity constraints**:  
      * *primary key*: the primary -key attributes
      * *foreign key + references*:  Some
      database systems, including MySQL, require an alternative syntax, “foreign key (dept name) references department(dept name)”, where the referenced attributes in the referenced table are listed explicitly.    
      * *not null*: `<attribute> <type> not null` he constraint excludes the null value from the domain of that attribute.   
        
  
!!! warning "Attention"    

    <center>SQL prevents any update to the database that violates an integrity constraint. If violates, SQL flags an error and prevents the update.<center>  
        
  
* **`drop table`**: delete a relation
    
!!! tip "Comparison"  

    <center> `drop table r` deletes the whole schema, including all tuples in the relation r. `delete from r` deletes all the tuples, yet retains relation r<center>  
      
  
* **`alter table`**: add attributes to an existing relation. All tuples in the relation are assigned null as the value for the new attribute. `alter table r add A D;` A is the name of the attribute to be added, and D is the type of the added attribute. Thus, `alter table r drop A;` deletes attribute A from relation r. To be aware, many database systems do not support dropping of attributes.  
  
------------------------------------    
  
  
## 3.3 Basic Structure of SQL Queries  

**The basic structure of an SQL query consists of three clauses: select, from, and where. A query takes as its input the relations listed in the from clause, operates on them as specified in the where and select clauses, and then produces a relation as the result.**  
  
### 3.3.1 Queries on a Single Relation  

* **select distinct**: In practice, duplicate elimination is time-consuming. Therefore, SQL allows duplicates in database relations as well as in the results of SQL expressions.1 Thus, the preceding SQL query lists each department name once for every tuple in which it appears in the instructor relation.  
* **select all**: SQL allows us to use the keyword all to specify explicitly that duplicates are not removed  
    
* The select clause may also contain arithmetic expressions involving the operators `+`,`−`, `∗`, and `/` operating on constants or attributes of tuples.  
  
* **where**: SQL allows the use of the logical connectives and, or, and not in the where clause.
The operands of the logical connectives can be expressions involving the comparison
operators <, <=, >, >=, =, and <>. SQL allows us to use the comparison operators
to compare strings and arithmetic expressions, as well as special types, such as date types.  
  
### 3.3.2 Queries on Multiple Relations   

* **Natural Join**:    
``` sql  
select name, course_id  
from instructor natural join teaches;  
```    
$instructor \bowtie teaches$    
> Danger in natural join  
> natural join may provide more constraints 
  
* **Rename**:   
> the `as` clause  
=== "as in select"  
  
    ``` sql  
    select ID, name, salary/12 as monthly_salary  
    from instructor;  
    ```  
      
=== "as in from"  
  
    ``` sql  
    select distinct T.name  
    from instuctor as T, instructor as S  
    where T.salary>S.salary and S.dept_name='Comp. Sci.';  
    ```  
      
  
         
            
* **Keyword as is optional and may be omitted.**  
> instructor as T = instructor T    
> Keyword as must be omitted in Oracle  
  
  
* **string operations**  
> SQL includes a string-matching operator for comparisons on character strings.  The operator “like” uses patterns that are described using two special characters:
>    * `%`: matches any substring of zero or more characters.  
>    * `_`: matches any single character.  
=== "example"  

    ``` sql  
    select name    
    from instructor  
    where name like '%dar%';  
    ```
=== "when we want to match the character % or _"  

    ``` sql  
    select name  
    from instructor  
    where name like '%\%%' escape '\';  
    ```  
  
  
  

* **Ordering the display of tuples**:  
   * **order by**: in alphabetical order  

   ``` sql   
   select name  
   from instructor  
   order by name;  
   ```   
   * We may specify desc for descending order or asc for ascending order, for each attribute; ascending order is the default. `order by name desc`  
   * can sort on multiple attributes    
   `order by dept_name, name`    

* **Specific where clause predicates**  
> **between**  
```sql  
select name 
from instructor  
where salary between 90000 and 100000;  
```  
> tuple comparison  
```sql  
select name, course_id  
from instructor, teaches  
where (instructor.ID, dept_name) = (teaches.ID, 'Biology');  
```   
  
* **set operations**  
> **union**  
```sql  
(select course_id from section where sem = ‘Fall’ and year = 2009) union (select course_id from section where sem = ‘Spring’ and year = 2010)  
```  
> **intersect**  
> **except**  
> Each of the above operations automatically eliminates duplicates
  
* To retain all duplicates use the corresponding multiset versions union all, intersect all and except all.  
    
      
* **Null Values**:  
> * It is possible for tuples to have a null value, denoted by null, for some of their attributes
  
> * The result of any arithmetic expression involving null is null
  
> * The predicate  is null can be used to check for null values.
  
* **(unknown or true) = true, (unknown or false) = unknown, (unknown or unknown) = unknown**  
* **(unknown and true) = unknown, (unknown and false) = false, (unknown and unknown) = unknown**  
* **(not unknown) = unknown**  
* `P is unknown` evaluates to true if predicate P evaluates to unknown.  
* Result of where clause predicate is treated as false if it evaluates to unknown  
  
  
* **Aggregate Functions**:  
> **group by**  
> **having**: after group by  
         
* **NULL**:  
> all aggregate operations except count(*) ignore tuples with null values on the aggregated attribute.  

> What if collection has only null values?  
> * count(*) returns 0 
> * all other aggregates return null   
  
* **Nested Subqueries**:   

> * A subquery is a select-from-where expression that is nested within another query.    
> **in**:  
```sql  
select distinct course_id  
from section  
where semester = 'Fall' and year = 2009 and course_id in (select course_id from section where semester = 'Spring' and year = 2010);  
```  
  
* **some**:  
    
```sql  
select name  
from instructor  
where salary > some (select salary from instructor where dept_name = 'Biology');  
```  
> =some is equivalent to the keyword in   
> yet not some is not equalvalent to not in  
   
* **all**:  
```sql  
select name  
from instructor  
where salary > all (select salary from instructor where dept_name = 'Biology');  
```  
> salary of the instructor is greater than all the salaries of instructors in the Biology department.  
> *!all is equalvalent to !in    
  
  
  
  
  
* **test for empty relations**  
> * **exists**:  
```sql   
select name from instructor where exists (select * from advisor where advisor.ID = instructor.ID);  
```  
> in a nested subquery, an attribute can be fetched from the outer query.    
```sql  
select distinct course_id  
from section as S  
where semester='Fall' and year=2009 and not exists (select * from section as T where semester='Spring' and year=2010 and S.course_id=T.course_id);  
```    

> * nstructor.ID  and S.course_id  are attributes that are not in the relation of the subquery, they are called correlation variables     

  
* **not exists to realize division**  
```sql  
select S.ID, S.name  
from student S  
where not exists((select course_id from section where dept_name='Biology') except (select course_id from takes as T where T.ID = S.ID));  
```    
  
* **test for absence of duplicate tuples**  
```sql  
select T.course_id  
from course as T  
where unique (select R.course_id  
from section as R  
where R.course_id = T.course_id and R.year = 2009);  
```  
> evaluates to 'true' on an empty set  
  
* **subqueries in the from clause**  
``` sql  
select dept_name, avg_salary  
from (select dept_name, avg(salary) as avg_salary  
      from instructor  
      group by dept_name)  
where avg_salary > 42000;  
```  
  
* **with clause**:  
> The with clause provides a way of defining a temporary view whose definition is available only to the query in which the with clause occurs. 
  
```sql
with max_budget(value) as 
(select max(budget) from department) select dept_name 
from department,max_budget  
where department.budget = max_budget_value;   
```  
  
* **complex queries using with clause**:  
```sql  
with dept_total(dept_name, value) as  
(select dept_name, sum(salary)  
 from instructor  
 group by dept_name)  
dept_total_avg(value) as  
(select avg(value)  
 from dept_total)  
select dept_name  
from dept_total, dept_total_avg  
where dept_total.value > dept_total_avg.value;  
```  
  
* **scalar subquery**:  
```sql  
select dept_name  
from department  
where budget = (select max(budget) from department);  
```    

### Modification of the Database  
* **deletion**:  

> delete from  
> delete from instructor where dept_name='Finance';  
  
* **insert one row**:  
=== "add a new tuple"  

    ```sql  
    insert into course  
        values('CS-437', 'Database Systems', 'Comp. Sci.', 4);  
    ```  
  
=== "add a new tuple to student with tot_creds set to null"  

    ```sql  
    insert into student  
        values('3003', 'Green', 'Finance', null);  
    ```    

=== "add all instructors to the instructor table"  

    ```sql  
    insert into instructor  
        select * from instructor_temp;  
    ```  
    * The select from where statement is evaluated fully before any of its results are inserted into the relation  
      
* **updates**  
=== "set"  

    ```sql  
    update instructor  
    set salary = salary * 1.10  
    where dept_name = 'Physics';  
    ```
    
=== "set multiple attributes"  

    ```sql    
    update instructor  
    set salary = salary * 1.10, dept_name = 'Finance'  
    where dept_name = 'Physics';  
    ```  
=== "case"  

    ```sql  
    update instructor  
    set salary = case  
                    when salary < 100000 then salary * 1.10  
                    else salary*1.03 
                 end;  
    ```
     
=== "updates with scalar subqueries"  

    ```sql  
    update student S  
      set tot_cred = (select sum(credits)  
                      from takes natural join course 
                      where S.ID=takes.ID and takes.grade <> 'F' and takes.grade is not null);  
    ```  
  

 
      
  
    
  
  














   

  





     


    

 
      







