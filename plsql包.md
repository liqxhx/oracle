
## 包简介
1. 包用于逻辑组合相关的PL/SQL类型(例如TABLE类型和RECORD类型)、PL/SQL项(例如游标和游标变量)和PL/SQL子程序(过程和函数)
2. 不仅简化了应用设计，提高了应用性能，而且还可以实现信息隐藏提高代码安全性
3. A package is a schema object that groups logically related PL/SQL types, variables, constants,   
    subprograms, cursors, and exceptions. A package is compiled and stored in the database, 
    where many applications can share its contents. 
    You can think of a package as an application.
4. 通过使用PL/SQL包，不仅简化了应用设计，提高了应用性能，而且还可以实现信息隐藏提高代码安全性。
5. PL/SQL包是代码组织方式，本身不包含程序代码，只是代码组织方式
6. 包由包头和包体两部分组成
7. 包头(package/package specification)：包头部分申明包内数据类型，常量，变量，游标，子程序和异常错误处理，这些元素为包的公有元素
    A package always has a specification, which declares the public items that can be referenced from outside the package. 
     You can think of the package specification as the application programming interface (API). 
8. 包主体(package body)：包主体则是包定义部分的具体实现，它负责为包头中所声明子程序提供具体的实现，在包主体中还可以声明包的私有元素
9. If the public items include cursors or subprograms, then the package must also have a body. 
    The body must define queries for public cursors and code for public subprograms. 
    The body can also declare and define private items that cannot be referenced 
    from outside the package, but are necessary for the internal workings of the package.
10. 包头和包主体分开编译，并作为两个分开的对象分别存放在数据库字典中
11. In either the package specification or package body, 
     you can map a package subprogram to an external Java or C subprogram by using a call specification, which maps the  
     external subprogram name, parameter types, and return type to their SQL counterparts
     
## 创建包头
```sql
create or replace package 包名
As | IS
    procedure 过程名();
    Function 函数名() return 数据类型;
    变量定义;
    异常定义;
    游标定义;
    ...........
    ...........
End 包名;
     
CREATE OR REPLACE PACKAGE pack_emp
IS
    TYPE emp_type IS RECORD(empNo NUMBER, salary NUMBER);

    function getNextNewEmpInfo return emp_type ;

    procedure AddNewEmp  (pi_first_name in VARCHAR2, 
                          pi_last_name in VARCHAR2，
                          return_code out VARCHAR2) ;

END pack_emp;
     
     
```

## 创建包体
```sql
create or replace Package Body 包名
As | IS
           Procedure 过程定义;
           Procedure 过程定义;
           Function 函数定义;
           Function 函数定义;
           .........;
end 包名;

create or replace package body pack_emp is
        function getNextNewEmpInfo return emp_type 
        IS 
        rec_emp emp_type; --私有变量
        begin
           SELECT MAX(t.employee_id)+1, AVG(t.salary) into rec_emp  FROM employees t;
           RETURN rec_emp;
        exception
           when others then
            dbms_output.put_line('exception occur in getNextNewEmpInfo');
            RETURN NULL;
        end getNextNewEmpInfo;
        
        PROCEDURE AddNewEmp(pi_first_name in VARCHAR2, pi_last_name in VARCHAR2, return_code out VARCHAR2) IS
        rec_new_emp emp_type; --私有变量
        begin
           rec_new_emp:=getNextNewEmpInfo;
           INSERT INTO employees_bak(employee_id,first_name,last_name,salary)  VALUES(rec_new_emp.empNo,pi_first_name,pi_last_name,rec_new_emp.salary);
           COMMIT;
           return_code:=0;
        exception
           when others then
              dbms_output.put_line('exception occur in AddNewEmp');
              return_code:=99;
        end AddNewEmp;
    end pack_emp;

 ```  
 
 ## 调用包中的子程序
1. 在同一包内调用:  
  当调用同一包内的其他组件时，可以直接调用，不需要加包名作为前缀  
2. 不在同一包内调用:  
  在包之外调用包内的组件时，需要加上包名和组件名(包名.组件名)  
3. 被其他用户调用:  
  当包被其他数据库调用时，必须在组件名前加用户名和报名作为前缀(用户名.包名.组件名)
  
  ## 包的查看与删除
1. 当建立了包之后，Oracle会将包名及其源代码信息存放到数据字典中。通过查询数据字典user_source，可以显示当前用户的包及其源代码
2. 当包不再需要时，可以删除包。如果只删除包体，那么可以使用命令`drop package body 包名`；如果同时删除包头和包体，那么可以使用命令`drop package 包名`
  
