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
  
## Integrity Constraints

* not null

``` sql

name varchar(20) not null
budget numeric(12,2) not null
```

* primary key
* unique

> form a candidate key, permit nulls, unlike primary key

* check(p)

> where P is a predicate. Specifies a predicate P that must be satisfied by every tuple in a relation.

### Referential Integrity

* cascade

> * delete cascade: delete all tuples in the referencing relation
> * update cascade: update all foreign keys in the referencing relation

### Complex Check Clauses

* subquery in check clause not supported by pretty much any database


### User-Defined Types

``` sql

create tyoe Dollars as numeric (12,2) final;
```


* **domains**:

> a type with constraints

``` sql
create domain degree_level varchar(10)
constraint degree_level_constaint
check (value in ('Bachelors', 'Masters', 'Doctorate'));
```

### large-object types

### Authorization

* **grant**:

``` sql
grant <privilege list>
on <relation name or view name>
to <user list>
```

### Revoking Authorization

``` sql
revoke <privilege list>
on <relation name or view name>
from <user list>
```


### role

* a role is a way to distinguish among users as far as what these users can access/update in the database

``` sql

create role <name>
grant <role> to <users>
```








