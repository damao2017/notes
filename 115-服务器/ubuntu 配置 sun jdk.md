# ubuntu 配置 sun jdk

1.下载文件并上传

以下都在超级用户下执行

2.解压

```
tar -zxvf 文件名
```

3.移动

```
mv jdk1.8.0_181/ /usr/lib/java
```

4.配置全局环境变量

```
vim /etc/profile
```

加入如下代码

```
#set jdk environment  
export JAVA_HOME=/usr/lib/java
export CLASSPATH=.:$JAVA_HOME/lib:$JAVA_HOME/jre/lib:$CLASSPATH  
export PATH=$JAVA_HOME/bin:$JAVA_HOME/jre/bin:$PATH 
```

测试

```
source命令通常用于重新执行刚修改的初始化文件，使之立即生效，而不必注销并重新登录。
source /etc/profile

java -version
```

 