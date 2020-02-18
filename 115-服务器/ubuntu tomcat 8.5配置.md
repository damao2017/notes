# ubuntu tomcat 8.5配置

配置好jdk以后，上传tomcat文件夹，直接可以运行，无需配置。

修改端口

打开/conf/server.xml，找到端口好8080，直接修改

开启https

1.生成证书

```
keytool -genkey -v -alias mykey -keyalg RSA -keystore mykey.keystore  -validity  36500
```

alias: 别名 这里起名testKey
keyalg: 证书算法，RSA
validity：证书有效时间，10年
keystore：证书生成的目标路径和文件名,替换成你自己的路径即可,我定义的是~/Lee/test.keystore

后面会要求输入一些信息，其中秘钥库口令和秘要口令最好输入同一个,并且记下这个口令,其他的随便填即可

2.将mykey.keystore放到tomcat/conf目录下

打开/conf/server.xml，找到8443这段(不同的tomcat配置文件不一样，这个是8.5的配置文件)

 

```
<Connector port="18443" protocol="org.apache.coyote.http11.Http11NioProtocol"
    maxThreads="150" SSLEnabled="true">
    <SSLHostConfig>
        <Certificate certificateKeystoreFile="conf/my.keystore"
            type="RSA" certificateKeystorePassword="wersdf234" />
    </SSLHostConfig>
 </Connector>
```

修改certificateKeystoreFile为我们保存的证书路径

增加certificateKeystorePassword属性，输入生成证书的密码

保存重启后即可访问https