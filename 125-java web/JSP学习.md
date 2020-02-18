# JSP学习

**JSP中的三种指令标签：**

| **指令**           | **描述**                                                |
| :----------------- | :------------------------------------------------------ |
| <%@ page ... %>    | 定义网页依赖属性，比如脚本语言、error页面、缓存需求等等 |
| <%@ include ... %> | 包含其他文件                                            |
| <%@ taglib ... %>  | 引入标签库的定义                                        |

 

**JSP注释：**

| **语法**       | 描述                                                 |
| :------------- | :--------------------------------------------------- |
| <%-- 注释 --%> | JSP注释，注释内容不会被发送至浏览器甚至不会被编译    |
| <!-- 注释 -->  | HTML注释，通过浏览器查看网页源代码时可以看见注释内容 |
| <\%            | 代表静态 <%常量                                      |
| %\>            | 代表静态 %> 常量                                     |
| \'             | 在属性中使用的单引号                                 |
| \"             | 在属性中使用的双引号                                 |

 

**JSP九个隐含对象：**

| **对象**    | **描述**                                                     |
| :---------- | :----------------------------------------------------------- |
| request     | **HttpServletRequest**类的实例                               |
| response    | **HttpServletResponse**类的实例                              |
| out         | **PrintWriter**类的实例，用于把结果输出至网页上              |
| session     | **HttpSession**类的实例                                      |
| application | **ServletContext**类的实例，与应用上下文有关                 |
| config      | **ServletConfig**类的实例                                    |
| pageContext | **PageContext**类的实例，提供对JSP页面所有对象以及命名空间的访问 |
| page        | 类似于Java类中的this关键字                                   |
| Exception   | **Exception**类的对象，代表发生错误的JSP页面中对应的异常对象 |

 

 **EL表达式**

| **隐含对象**     | **描述**                      |
| :--------------- | :---------------------------- |
| pageScope        | page 作用域                   |
| requestScope     | request 作用域                |
| sessionScope     | session 作用域                |
| applicationScope | application 作用域            |
| param            | Request 对象的参数，字符串    |
| paramValues      | Request对象的参数，字符串集合 |
| header           | HTTP 信息头，字符串           |
| headerValues     | HTTP 信息头，字符串集合       |
| initParam        | 上下文初始化参数              |
| cookie           | Cookie值                      |
| pageContext      | 当前页面的pageContext         |

 

 例如：${sessionScope.username}是取出Session范围的username 变量。这种写法是不是比之前JSP 的写法：

String username =(String) session.getAttribute("username");容易、简洁许多.

 假若我们在cookie 中设定一个名称为userCountry的值，那么可以使用${cookie.userCountry}来取得它。



 web.xml 

```xml
<context-param>
    <param-name>userid</param-name>
    <param-value>mike</param-value>
</context-param>
```

使用${initParam.userid}来取得名称为userid，其值为mike 的参数。

之前的做法：String userid =(String)application.getInitParameter("userid");

 使用param和paramValues两者来取得数据。
${param.name}
${paramValues.name}

这里param 的功能和request.getParameter(String name)相同，而paramValues和
request.getParameterValues(String name)相同。如果用户填了一个表格，表格名称为username，则我们就可以使用${param.username}来取得用户填入的值。

 **如果需要在支持表达式语言的页面中正常输出“$”符号，则在“$”符号前加转义字符“\”，否则系统以为“$”是表达式语言的特殊标记。**

```
<!-- 计算三目运算符 -->
<tr>
    <td>\${(1==2) ? 3 : 4}</td>
    <td>${(1==2) ? 3 : 4}</td>
</tr> 
```

**注意：在使用EL 关系运算符时，不能够写成：
${param.password1} = =${param.password2}
或者
${ ${param.password1 } = = ${param.password2 } }
而应写成
${ param.password1 = =param.password2 }**

 