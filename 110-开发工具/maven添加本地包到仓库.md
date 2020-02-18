# maven添加本地包到仓库

## eclipse中增加jar包

1.下载相应的jar包，此处我使用ojdbc5.jar（maven仓库中不可以下载）为例，记录使用方法；

2.右击项目——>Run AS/Debug As——>Maven Build.. 进入命令使用界面，如下图：

3.在Goals里面输入：

```
install:install-file -Dfile=D:\ojdbc5.jar -DgroupId=com.oracle -DartifactId=ojdbc5 -Dversion=1.0 -Dpackaging=jar
```

 

-Dfile：是安装ar包的路径；
-DgroupId：是jar包在maven中的pom.xml中的依赖形式中的groupId；
-DartifactId：是jar包在maven中的pom.xml中的依赖形式中的artifactId；
-Dversion：是jar包在maven中的pom.xml中的依赖形式中的version；

4.在pom.xml中引用

```xml
<dependency>
    <groupId>com.oracle</groupId>
    <artifactId>ojdbc5</artifactId>
    <version>1.0</version>
</dependency>﻿​
```

## 使用maven命令添加

```bash
mvn install:install-file  -Dfile=D:\ojdbc5.jar  -DgroupId=com.oracle  -DartifactId=ojdbc5 -Dversion=1.0 -Dpackaging=jar
```

 