# tomcat数据库连接池

 在项目的META-INF目录下，新增context.xml文件 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Context>
<Resource 
        name="jdbc/mysql/TestDB"
        driverClassName="com.mysql.jdbc.Driver"
        url="jdbc:mysql://192.168.253.132:13306/testDB?useSSL=false"
        username="test"
        password="test1"
        maxActive="100"
        maxIdle="30"
        maxWait="10000"
        author="Container"
        type="javax.sql.DataSource"
    />
</Context>
```

 author="Container" 说明是由tomcat管理还是由应用程序管理。 

 程序代码： 

```java
Context ctx = new InitialContext();
//字符串“java:comp/env/”是固定的，后面跟xml配置的resource的name属性
DataSource ds = (DataSource) ctx.lookup("java:comp/env/jdbc/mysql/TestDB");
Connection connection = ds.getConnection();
//后面就是普通的操作
```

