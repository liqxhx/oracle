# oracle

## 数据库服务器安装
略
## 实例创建
略
## 安装客户端
略
## 连接
sqlplus / as sysdba<br/>
sqlplus user/pwd@//host:port/sid [as sysdba]<br/>
sqlplus /nolog<br/>
conn user/pwd as sysdba<br/>

## 查看数据库版本
select * from v$version<br/>

## 查看sqlplus版本及位数
sqlplus -v<br/>

## 启动关闭实例
startup<br/>
startup force 强制重启<br/>
startup nomount pfile='/path/initSID.ora'<br/>
alter database mount<br/>
alter database open<br/>

shutdown abort/immediate/transaction/normal<br/>

## 中文乱码
出现中文乱码的主要原因是字符集不同。<br/>
在Oracle中，我们关心三个地方的字符集：<br/>
* Oracle服务器内部的字符集
* NLS_LANG变量里保存的字符集
* 客户端应用的字符集
NLS_LANG=SIMPLIFIED CHINESE_CHINA.ZHS16GBK<br/>





