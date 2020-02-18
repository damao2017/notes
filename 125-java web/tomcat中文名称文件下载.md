# tomcat中文名称文件下载

tomcat内置的对于get协议中 的URL编码都是ISO-8859-1

这个字符集不能直接支持中文等双字节的信息

而中文文件的下载链接恰恰是通过get协议进行的。

方法1：

```xml
  <!--  打开config/server.xml文件，增加URIEncoding="utf-8"到8080这个端口  -->
  <Connector port="8080"
             URIEncoding="utf-8"
             protocol="HTTP/1.1"
             connectionTimeout="20000"
             redirectPort="8443" />
```

 方法2： 

```java
//将字符串重新编码即可，推荐这种方式
String filename = object.getNo()+"-"+object.getName();
filename = new String(filename.getBytes("UTF-8"), "ISO-8859-1");

resp.setHeader("Content-Disposition", "attachment;filename="+filename);
```

