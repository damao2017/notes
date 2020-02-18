# web项目路径

**请求转发路径**

```
req.getRequestDispatcher("/index.jsp").forward(req, resp);
```

/代表项目跟目录

 

**html资源路径**

`<img src="/aaa.jpg" />`

`<a href="/aaa.html" ></a>`

`javascript,css资源等`

/代表tomcat根目录

 

**重定向路径**

`resp.sendRedirect("/UserSystem/main.jsp");`

/代表tomcat根目录