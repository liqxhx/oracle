[ref](https://docs.oracle.com/cd/E11882_01/appdev.112/e25519/composites.htm#LNPLS00501)
### 复合数据类型
```sql
    declare
       v_empid1 number(6);
       v_empid2 employees.employee_id%type; 
       type emp_record_type is record(empid employees.employee_id%type,
                                     firstname employees.first_name%type,
                                     salary employees.salary%type);
      v_emp1 emp_record_type;
      v_emp2 employees%rowtype;
    begin
       select employee_id into v_empid1 from employees where rownum<=1;
       select employee_id into v_empid2 from employees where rownum<=1;
       dbms_output.put_line('v_empid1:'||v_empid1);
       dbms_output.put_line('v_empid2:'||v_empid2);
       select employee_id,first_name,salary into v_emp1 from employees  where rownum<=1;
       select * into v_emp2 from employees where rownum<=1;
      dbms_output.put_line('v_emp1:'||v_emp1.empid);
      dbms_output.put_line('v_emp2:'||v_emp2.employee_id);
    end;

    
```
### Collection-索引表

* 索引表也被称为PL/SQL表，定义方式如下:
> TYPE type_name IS TABLE OF element_type INDEX BY key_type;
* 索引表的下标可以为正数，可以为负数，而且可以为VARCHAR2等非数字型
* 索引表只能作为PL/SQL复合数据类型使用，而不能作为表列的数据类型使用
```sql
 declare 
       type emp_record_type is record(empid employees.employee_id%type,
                                    firstname employees.first_name%type,
                                     salary employees.salary%type);

        type emp_table_type is table of emp_record_type index by pls_integer; 
        --type emp_table_type is table of number index by varchar(2); 相当于键值对
        v_emp_table emp_table_type;
        begin
         select t.employee_id,t.first_name,t.salary bulk collect into v_emp_table 
         from employees t where rownum<=10;
   
         for i in 1..v_emp_table.count loop
            dbms_output.put_line(v_emp_table(i).firstname);
            
         end loop;
         
         --v_emp_table('JAMES') := 20;
         --v_emp_table('XXX')   := 21;
         --dbms_output.put_line(v_emp_table('JAMES'));
  end;
        
        
```



### Collection-嵌套表
* 嵌套表也是一种用于处理PL/SQL数组的数据类型。嵌套表的元素下边从1开始，并且元素个数没有限制。
* 嵌套表类型不仅可以作为PL/SQL复合数据类型使用，而且可以作为表列的数据类型使用
* 嵌套表定义方式如下:
> TYPE type_name IS table of element_type;

```sql
 declare 
       type emp_record_type is record(empid employees.employee_id%type,
                                    firstname employees.first_name%type,
                                     salary employees.salary%type);

        type emp_table_type is table of emp_record_type; --index by pls_integer
        v_emp_table emp_table_type;
  begin
         select t.employee_id,t.first_name,t.salary bulk collect into v_emp_table 
         from employees t where rownum<=10;
   
   --for i in v_emp_table.first..v_emp_table.last loop
         for i in 1..v_emp_table.count loop
            dbms_output.put_line(v_emp_table(i).firstname);
            --v_emp_table.delete(i);
         end loop;
  end;
    
```
* 独立的嵌套表类型(standalone nested table type)，作为数据库对象存储在数据库中，然后再用它去定义变量
> CREATE OR REPLACE TYPE nt_type IS TABLE OF NUMBER;
```sql
CREATE OR REPLACE TYPE nt_type IS TABLE OF NUMBER;

declare
  v_sal nt_type;  
begin
  --select salary into v_sal from employees;多行时要加bluk collect
  select salary BLUK COLLECT into v_sal from employees;
  for i in 1..v_sal.count loop
    dbms_output.put_line(v_sal(i));--一个字段时不用v_sal(i).字段名，可以省略
  end loop;
end;
```
还可以这么用

```sql
CREATE OR REPLACE TYPE nt_class IS OBJECT(id number,name varchar2(20));--就是record
CREATE OR REPLACE TYPE nt_type IS TABLE OF nt_class; --多行多列

```
### Collection-变长数组

* VARRAY也是一种用于处理PL/SQL数组的数据类型，它也可以作为表列的数据类型使用。该数据类型与高级语言数组非常类似，其元素下标从1开始，并且元素的最大个数是有限制的。
* VARRAY类型不仅可以作为PL/SQL复合数据类型使用，而且可以作为表列的数据类型使用
```sql
    TYPE type_name is VARRAY(size) of element_type;
    declare 
     type emp_record_type is record(empid employees.employee_id%type,
                                    firstname employees.first_name%type,
                                    salary employees.salary%type);

     type emp_table_type is varray(10) of emp_record_type;
     v_emp_table emp_table_type;
    begin
         select t.employee_id,t.first_name,t.salary bulk collect into v_emp_table  from employees t where rownum<=10;
         for i in 1..v_emp_table.count loop
              dbms_output.put_line(v_emp_table(i).firstname);
        end loop;
      end;
 ```     
 
### 对象类型 
* Oracle PL/SQL支持面向对象编程，面向对象类型是基于对象类型来完成的。对象类型是用户自定义的一种复合数据类型。
 ```sql
create or replace type emp_obj as object(empid number(6),first_name varchar2(30),salary number(8,2));
create or replace type emp_obj_list is table of emp_obj;
    declare
        v_emp emp_obj;
        v_emp_list emp_obj_list;
     begin
        v_emp:=emp_obj(198,'Donald',88.00);
        select emp_obj(employee_id,first_name,salary) into v_emp from employees where rownum<=1;
        select  emp_obj(employee_id,first_name,salary)  bulk collect into v_emp_list from employees 
       where rownum<=10;
     end;
 ``` 
