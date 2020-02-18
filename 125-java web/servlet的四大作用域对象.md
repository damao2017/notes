# servlet的四大作用域对象

**ServletContext域**

application，作用范围是整个Web应用。当Web应用被加载进容器时创建代表整个web应用的ServletContext对象，当服务器关闭或Web应用被移除时，ServletContext对象跟着销毁。

 

**HttpSession域**

作用范围是一次会话。生命周期是在第一次调用request.getSession()方法时，服务器会检查是否已经有对应的session,如果没有就在内存中创建一个session并返回。当一段时间内session没有被使用（默认为30分钟），则服务器会销毁该session。如果服务器非正常关闭（强行关闭），没有到期的session也会跟着销毁。如果调用session提供的invalidate（） ，可以立即销毁session。

 

**ServletRequest域**

作用范围是整个请求链（请求转发也存在）；生命周期是在service方法调用前由服务器创建，传入service方法。整个请求结束，request生命结束。

 

**PageContext 域**

1、生命周期：当对JSP的请求时开始，当响应结束时销毁。

2、作用范围：（页面范围）整个JSP页面，是四大作用域中最小的一个。

作用：

（1）获取其它八大隐式对象，可以认为是一个入口对象。

（2）获取其所有域中的数据

pageContext　　操作所有域中属性的方法　　　　　　　　　　

```
public java.lang.Object getAttribute(java.lang.String name,int scope) 　　　　　　　　　　　　　
public void setAttribute(java.lang.String name, java.lang.Object value,int scope)
public void removeAttribute(java.lang.String name,int scope)
pageContext 中代表域的常量
PageContext.APPLICATION_SCOPE
PageContext.SESSION_SCOPE
PageContext.REQUEST_SCOPE
PageContext.PAGE_SCOPE﻿​  
```

findAttribute方法,在四大域中搜寻属性，搜寻的顺序是page域、request域、session域、application域，从小域到大域开始搜索，如果搜索到就直接获取该值，如果所有域中都找不到，返回一个null(与el表达式不同，此处返回null，对网页是不友好的)

（3）跳转到其他资源 其身上提供了forward和include方法，简化重定向和转发的操作