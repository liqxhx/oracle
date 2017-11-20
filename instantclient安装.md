

## 下载软件包

1. [Basic: All files required to run OCI, OCCI, and JDBC-OCI applications](http://www.oracle.com/technetwork/database/features/instant-client/index-097480.html)
2. [SQL*Plus: Additional libraries and executable for running SQL*Plus with Instant Client](http://www.oracle.com/technetwork/database/features/instant-client/index-097480.html)

## 解压
解压到同一个目录下

## 配置环境变量
```shell
export ORACLE_HOME=/home/liqxhx/instantclient_12_2
#可选
#export TNS_ADMIN=$ORACLE_HOME
#可选
#export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$ORACLE_HOME
#NLS_LANG=SIMPLIFIED CHINESE_CHINA.ZHS16GBK
```

## FAQ
1. ubuntu16.04下报找不到libaio.so1
sudo apt install libaio1
