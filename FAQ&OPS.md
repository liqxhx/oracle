### 0、de

### 1、truncate表后，无法使用oracle的闪回特性恢复被删除的数据，使用闪回时报错：ORA01466
```
truncate table xxx; truncate是DDL语句

```

### 2、闪回查询
```
使用闪回查询
select * from xxx as of timestamp to_timestampe('2018-03-25 21:00:00', 'yyyy-mm-dd hh24:mi:ss') ;

```

### 3、使用闪回表到某个时间点
```
alter table xxx enable row movement;--在flashback中使用，当需要使用flashback table功能时，需要首先打开row mvoement的选项，否则会报错
flashback table xxx to timestamp to_timestampe('2018-03-25 21:00:00', 'yyyy-mm-dd hh24:mi:ss') ;
alter table xxx disable row movement;
```

### 4、查询DML发生时间
```
SELECT VERSIONS_STARTTIME,VERSIONS_ENDTIME,VERSIONS_XID,VERSIONS_OPERATION 
FROM XXX
VERSIONS BETWEEN TIMESTAMP MINVALUE AND MAXVALUE 
--WHERE VERSIONS_STARTTIME IS NOT NULL 
ORDER BY VERSIONS_STARTTIME-- DESC;
```

### 5、闪回被drop的表
```
select * from recyclebin;
flashback table xxx to before drop;
```

### 6、oracle中null=null为false(unkown)； null is null为true

### 7、not in
```
not in 等价于 != All
select * from t where xx not in(collections);
等价于
select * from t where xx != v1 and xx != v2 ...;

collections中如果有一条为null,整个查询为空
```
### 8、行转列
name|subject|score
-|-|-|
zs|语文|90
zs|数学|90
zs|英文|90
ls|语文|92
ls|数学|92
ls|英文|92

行转列后


name|chinese|math|english
-|-|-|-|
zs|90|90|90
ls|92|92|92

```
有三种方法
select name,wm_concat(score) from score group by name
;

select name,
max(decode(subject, '语文', score, null)) "语文成绩", --max(case when subject = '语文' then score else null end) "语文成绩"
max(decode(subject, '数学', score, null)) "数据成绩",
max(decode(subject, '英语', score, null)) "英语成绩",
from score
group by name
order by name
;
--11g后，可以用专门的函数piovt

select * from score
piovt(max(score) for subject in('语文','数学','英语')
order by name
;
```
### 9、列转行
```
union

select * from score2
unpivot(socre for subject in(chinese as "语文", math as "数学", english as "英语"))
```
