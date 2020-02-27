# ubuntu 安装nexus oss

nexus3依赖jdk8

先安装jdk8

然后解压

```
$mkdir nexus
$tar -zxvf nexus-3.15.2-01-unix.tar.gz﻿​-C nexus
$tree -L 1
.
├── nexus-3.15.2-01
└── sonatype-work

2 directories, 0 files

修改端口
~/soft/nexus/nexus-3.15.2-01/etc$ vim nexus-default.properties 
将8081改成18081

启动
~/soft/nexus/nexus-3.15.2-01/bin$ ./nexus run

修改linux最大文件，要不然nexus会有告警
vim /etc/security/limits.conf
增加,ntkd为启动他的用户
ntkd - nofile 65536
或者 任意用户任何类型
* - nofile 65536
```

