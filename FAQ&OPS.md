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
