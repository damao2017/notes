### Log4j Slf4j日志学习

## Slf4j

使用Slf4j+Log4j组合，采用门面模式，以后换日志工具，直接替换底层的日志工具即可。

```xml
<dependency>
	<groupId>org.slf4j</groupId>
	<artifactId>slf4j-log4j12</artifactId>
	<version>1.7.25</version>
</dependency>
```

artifactId说明
slf4j-log4j12:使用slf4j和log4j组合
slf4j-jdk14：使用slf4j和jdk日志组合

使用

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class Slf4jTest {
	private static Logger logger = LoggerFactory.getLogger(Slf4jTest.class);
	public static void main(String[] args) {
		logger.info("name : {}, Time: {}", "smallming", System.currentTimeMillis());
		logger.info("Current Time: " + System.currentTimeMillis());
		logger.trace("trace log");
		logger.warn("warn log");
		logger.debug("debug log");
		logger.info("info log");
		logger.error("error log");
	}
```

## Log4j

log4j.properties

```bash
log4j.rootLogger=debug, stdout
#, logfile 
log4j.appender.stdout=org.apache.log4j.ConsoleAppender 
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout 
log4j.appender.stdout.layout.ConversionPattern=%d{yyyy/MM/dd HH:mm:ss,SSS} %p [%l] - %m%n 
```

 输出 

```
 2018/11/07 18:40:47,888 ERROR [com.ntkd.test.Slf4jTest.main(Slf4jTest.java:21)] - error log﻿​
```

 日志信息格式中几个符号所代表的含义：
-X号: X信息输出时左对齐；
%p: 输出日志信息优先级，即DEBUG，INFO，WARN，ERROR，FATAL,
%d: 输出日志时间点的日期或时间，默认格式为ISO8601，也可以在其后指定格式，比如：%d{yyy MMM dd HH:mm:ss,SSS}，输出类似：2002年10月18日 22：10：28，921
%r: 输出自应用启动到输出该log信息耗费的毫秒数
%c: 输出日志信息所属的类目，通常就是所在类的全名
%t: 输出产生该日志事件的线程名
%l: 输出日志事件的发生位置，相当于%C.%M(%F:%L)的组合,包括类目名、发生的线程，以及在代码中的行数。举例：Testlog4.main (TestLog4.java:10)
%x: 输出和当前线程相关联的NDC(嵌套诊断环境),尤其用到像java servlets这样的多客户多线程的应用中。
%%: 输出一个"%"字符
%F: 输出日志消息产生时所在的文件名称
%L: 输出代码中的行号
%m: 输出代码中指定的消息,产生的日志具体信息
%n: 输出一个回车换行符，Windows平台为"/r/n"，Unix平台为"/n"输出日志信息换行 



日志级别：

| 级别  | 值   |
| :---- | :--- |
| FATAL | 0    |
| ERROR | 3    |
| WARN  | 4    |
| INFO  | 6    |
| DEBUG | 7    |

 log4j可以制定某个包，某个类输出日志 

```bash
log4j.rootLogger=error, stdout

#com.ntkd.usermapper.UserMapper为UserMapper.xml里面的
#mapper.namespace和下面id的组合
log4j.logger.com.ntkd.usermapper.UserMapper=debug

#com.ntkd.test.Slf4jTest为类的全名称
log4j.logger.com.ntkd.test.Slf4jTest=debug

log4j.appender.stdout=org.apache.log4j.ConsoleAppender 
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout 
log4j.appender.stdout.layout.ConversionPattern=%d{yyyy/MM/dd HH:mm:ss,SSS} %p [%l] - %m%n﻿​
```

