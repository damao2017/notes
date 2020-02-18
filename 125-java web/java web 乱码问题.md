# java web 乱码问题

### **乱码问题**

具体是因为Tomcat默认是按ISO-8859-1进行URL解码，ISO-8859-1并未包括中文字符，这样的话中文字符肯定就不能被正确解析了。

**通用方式**

String name = req.getParameter("name");

name = new String(name.getBytes("ISO-8859-1"),"UTF-8");

 

**POST方式（因为post提交数据是在http实体里面）**

req.setCharacterEncoding("UTF-8");

 

**GET方式（因为get提交数据是在utl里面）**

req.setCharacterEncoding("UTF-8");

tomcat-conf-server.xml

找到

```
<Connector connectionTimeout="20000" port="8080" protocol="HTTP/1.1" redirectPort="8443"/> 
```

 加入属性`URIEncoding`或者`useBodyEncodingForURI`

```
<Connector connectionTimeout="20000" port="8080" protocol="HTTP/1.1" redirectPort="8443" useBodyEncodingForURI="true"/>
```

 useBodyEncodingForURI参数表示是否用request.setCharacterEncoding 参数对URL提交的数据和表单中GET方式提交的数据进行重新编码，在默认情况下，该参数为false。

 URIEncoding参数指定对所有GET方式请求进行统一的重新编码（解码）的编码。

 URIEncoding和useBodyEncodingForURI区别是：

URIEncoding是对所有GET方式的请求的数据进行统一的重新编码，

useBodyEncodingForURI则是根据响应该请求的页面的request.setCharacterEncoding参数对数据进行的重新编码，不同的页面可以有不同的重新编码的编码