# Tomcat热部署

打开Server.xml，找到

```xml
<Host appBase="webapps" autoDeploy="true" name="localhost" unpackWARs="true">
 <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs" pattern="%h %l %u %t "%r" %s %b" prefix="localhost_access_log" suffix=".txt"/>
</Host>
```

在`Host`标签加入：

```xml
<Context docBase="E:\apache-tomcat-8.5.32\webapps\ServletDemo" path="/ServletDemo" reloadable="true" />
```

`docBase`：项目绝对路径

`docBase`：tomcat路径

`reloadable`：自动加载true