# Intermediate SQL  

## Join Expressions  

### Join Relations   

* **Join type**:    

> * defines how tuples in each relation that do not match any tuple in the other relation (based on the join condition) are treated.   

* **join condition**:  
> * defines which tuples in the two relations match, and what attributes are present in the result of the join.   

=== "course natural right outer join prereq"    

    ```sql  
    SELECT *  
    FROM course NATURAL RIGHT OUTER JOIN prereq;  
    ```  
    * keep all prereq tuples  
    
=== "course natural full outer join prereq"  
  
    ```sql  
    SELECT *  
    FROM course NATURAL FULL OUTER JOIN prereq;  
    ```  
    * keep all course and prereq tuples   

=== "course full outer join prereq using (couse_id)"  

    ```sql  
    SELECT *  
    FROM course FULL OUTER JOIN prereq USING (course_id);  
    ```  

=== "course inner join prereq on course.course_id = prereq.course_id"  

    ```sql  
    SELECT *  
    FROM course INNER JOIN prereq ON course.course_id = prereq.course_id;  
    ```  

=== "course left outer join prereq on course.course_id = prereq.course_id"  

    ```sql  
    SELECT *  
    FROM course LEFT OUTER JOIN prereq ON course.course_id = prereq.course_id;  
    ```  

  
  
  
## Transactions  
* **commit work**  
* **rollback work**  
  
## Views   

* to disguise certain daya from the view of certain users    
* **create view**   

```sql  

   create view faculty as  
     select *  
     from instructor  
```  
> * then we can use it  
> * views can be defined by other views  
> * updates of a view  
> * Most SQL implementations allow updates only on simple views 
  
  
* **materialized view**   

> * create a physical table containing all the tuples in the result of the query defining the view
  
* **index creation**:   

```sql    
create index idx_course_id on course(course_id);  
```  









