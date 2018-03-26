## 查看用户信息
```
show user
用户信息字典dab_users
```
## DB级默认配置
```
SELECT * FROM Database_Properties
```
## 查建ddl语句
```
select dbms_metadata.get_ddl('TABLE','xxx') from dual; 
```
## 注释 
user_tab_comments  
user_col_comments  
最近接手配合另一个项目改造，需要从新系统的表拉字段到我们的系统，由于没有比较规则的文档可参考，只能这样猜了。还好有注释，反正慢慢搞...ing
```
create table 表名(字段列表);
comment on table 表名 is '表注释';
comment on column 表名.字段名1 is '字段1注释';
...
comment on column 表名.字段名N is '字段N注释';

select t.comments,c.*
from user_tab_comments t left join on user_col_comments c 
on t.table_name = c.table_name
where t.comments like '%xxx%'
```
## 表、列、约束
user_tab_columns  
user_tab_comments  
user_col_comments  
user_cons_columns  
user_constraints

```
select tcc.comments "表",
       tc.column_name "列名",
       cc.comments "列描述",
       tc.data_type||case tc.data_type 
          when 'VARCHAR2' then '('||tc.data_length||')'
          when 'NUMBER' then '('||tc.data_precision||','||tc.data_scale||')'
          else '' 
          end "类型",
       tc.nullable "是否为空",
       uc.constraint_type "约束",
from user_tab_columns tc
left join user_tab_comments tcc on tc.table_name = tcc.table_name
left join user_col_comments cc on tc.table_name = cc.table_name and tc.column_name = cc.column_name
left join user_cons_columns ucc on tc.table_name = ucc.table_name and tc.column_name = ucc.column_name
left join user_constraints uc on ucc.constraint_name = uc.constraint_name
where tc.table_name = '&table_name'
order by tc.column_id
```
## 表空间使用率
```
SELECT t.tablespace_name,
       t.total,
       t.total-f.free "used(m)",
       f.free,
       (t.total-f.free)/t.total*100 "use rate(%)"
FROM
       (SELECT tablespace_name,SUM(bytes)/1024/1024 total
               FROM dba_data_files GROUP BY tablespace_name )t,
       (SELECT  tablespace_name,SUM(bytes)/1024/1024 free
                FROM dba_free_space GROUP BY tablespace_name )f
WHERE t.tablespace_name = f.tablespace_name
AND t.tablespace_name = 'xxx'      
```

## 其它
```
SELECT * FROM v$DATABASE 
;
SELECT * FROM v$instance
;
SELECT * FROM v$session 
where machine like upper('%xxx%')
--
;
SELECT *　FROM v$controlfile
;
SELECT * FROM v$parameter
;
SELECT * FROM v$log
;
SELECT * FROM v$logfile
;
/*ARCHIVE LOG LIST
show parameter control_files;
show parameter pfile;

*/
;
SELECT * FROM v$locked_object
;
SELECT * FROM v$version
/*Oracle Database 11g Enterprise Edition Release 11.2.0.4.0 - 64bit Production
PL/SQL Release 11.2.0.4.0 - Production
CORE  11.2.0.4.0  Production
TNS for Linux: Version 11.2.0.4.0 - Production
NLSRTL Version 11.2.0.4.0 - Production*/

;
SELECT * FROM dba_users
where username = upper('cfssit')
;
-- contents为表空间类型
SELECT * FROM dba_tablespaces 
where tablespace_name = 'xxx'
;
SELECT * FROM dba_data_files
where tablespace_name = 'xxx'
;
SELECT * FROM dba_temp_files
where tablespace_name = 'TEMP2'
;



;
SELECT * FROM user_tables
where table_name like '%ADMIN%' 
```

