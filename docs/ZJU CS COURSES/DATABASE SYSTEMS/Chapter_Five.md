# Advanced SQL

* The SQL standard defines embeddings of SQL in a variety of programming languages such as C, Java, and Cobol.
* A language to which SQL queries are embedded is referred to as a host language, and the SQL structures permitted in the host language comprise embedded SQL.
* Main issues: exchange of parameters and results between the host language and SQL statements; sets vs. variables; get the execution status of the SQL statements; compile to host language;

```c
EXEC SQL update instructor set salary=salary*1.05 where salary<:some_amount END_EXEC;//:some_amount is a host variable
```c
EXEC SQL
   declare c cursor for     
select ID, name
from student
where tot_cred > :credit_amount
END_EXEC
```

* **open**: open the cursor

```c
EXEC SQL open c END_EXEC
```

* **fetch**: fetch the next row from the cursor

```c
EXEC SQL fetch c into :student_id, :student_name END_EXEC
```

> Repeated calls to fetch get successive tuples in the query result
> A variable called SQLSTATE in the SQL communication area (SQLCA) gets set to ‘02000’ to indicate no more data is available

* **close**: close the cursor

```c
EXEC SQL close c END_EXEC
```

* **example**:

```c
void getStudentInfo()
{
	int credit_amount;
	char sId[16];
	char sName[16];
	EXEC SQL declare c cursor select id, name from student where tot_cred> :credit_amount END EXEC;
	printf("Please input the credit amount: ");	
	scanf("%d",&credit_amount);
	EXEC SQL open c END_EXEC;
	while (1)
	{
		EXEC SQL fetch c into :sId, :sName END_EXEC;
		if (!strcmp(SQLSTATE,"02000"))
			break;
		printf("%s %s\n",sId,sName);
	}
	EXEC SQL close c END_EXEC;
}
```

* **updates through cursors**:

```c
delcare c cursor for
  select *
  from instructor
  where dept_name='Music'
for update
```

```c
update instructor
set salary=salary+100
where current of c
```

* **ODBC and JDBC**:
!!! note "illustration"  

    ![alt text](<Screenshot 2025-03-18 at 11.45.11.png>)

* **Driver and DSN configuration**:

```c
int ODBCexample()
{
  RETCODE error;
  HENV    env;     /* environment */ 
  HDBC    conn;  /* database connection */ 
  SQLAllocEnv(&env);
  SQLAllocConnect(env, &conn);
  SQLConnect(conn, “Mike", SQL_NTS, "avi", SQL_NTS, "avipasswd", SQL_NTS); 
  { …. Do actual work , usually use a statement handle to do the querey… }

  SQLDisconnect(conn); 
  SQLFreeConnect(conn); 
  SQLFreeEnv(env); 
}
```




