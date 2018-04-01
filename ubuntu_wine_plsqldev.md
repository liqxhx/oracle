## 安装winetricks

## 配置wine
```
记录下任意一个"驱动器"映射的目录，目的是将instantclient安装到它下面
c: -> ../drive_c
即
c: -> /home/xxx/.wine/drive_c
```

## 安装instantclient下
```

/home/xxx/.wine/drive_c/instantclient_12_1
```

## 再次配置wine 
```
wine regedit

Path = C:\windows;C:\windows\system;C:\instantclient_12_1
TNS_ADMIN = c:\instantclient_12_1\admin
ORACLE_HOME = c:\instantclient_12_1
NLS_LANG = SIMPLIFIED CHINESE_CHINA.ZHS16GBK
```

## plsqldev配置
```
oracle home指定为：c:\instantclient_12_1
oci library为：c:\instantclient_12_1\oci.dll
```
