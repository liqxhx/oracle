# oracle
--------------------------------------------------------
# 数据库服务器安装
略
# 实例创建
略
# 安装客户端
略
# 连接
sqlplus / as sysdba
sqlplus user/pwd@//host:port/sid [as sysdba]
sqlplus /nolog
conn user/pwd as sysdba

# 查看数据库版本
select * from v$version

# 查看sqlplus版本及位数
sqlplus -v

# 启动关闭实例
startup
startup force 强制重启
startup nomount pfile='/path/initSID.ora'
alter database mount
alter database open

shutdown abort/immediate/transaction/normal

# 中文乱码
出现中文乱码的主要原因是字符集不同。
在Oracle中，我们关心三个地方的字符集：
Oracle服务器内部的字符集
NLS_LANG变量里保存的字符集
客户端应用的字符集
NLS_LANG=SIMPLIFIED CHINESE_CHINA.ZHS16GBK





