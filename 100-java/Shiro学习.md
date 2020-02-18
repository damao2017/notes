### Shiro学习

```xml
<!-- 依赖的jar -->
<dependency>
	<groupId>org.apache.shiro</groupId>
	<artifactId>shiro-core</artifactId>
	<version>1.3.2</version>
</dependency>
<dependency>
	<groupId>org.slf4j</groupId>
	<artifactId>slf4j-log4j12</artifactId>
	<version>1.7.25</version>
</dependency>
```

```ini
#shiro-first.ini
[users]
zhangsan=111
lisi=222
```

```java
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.config.IniSecurityManagerFactory;
import org.apache.shiro.mgt.SecurityManager;
import org.apache.shiro.subject.Subject;
import org.apache.shiro.util.Factory;

public class Test {

	public static void main(String[] args) {
		// 创建securityManager工厂，通过ini配置文件创建securityManager工厂
				Factory<SecurityManager> factory = new IniSecurityManagerFactory("classpath:shiro-first.ini");
				// 创建SecurityManager
				SecurityManager securityManager = factory.getInstance();
				// 将securityManager设置当前的运行环境中
				SecurityUtils.setSecurityManager(securityManager);
				// 从SecurityUtils里边创建一个subject
				Subject subject = SecurityUtils.getSubject();
				// 在认证提交前准备token（令牌）
				// 这里的账号和密码 将来是由用户输入进去
				UsernamePasswordToken token = new UsernamePasswordToken("zhangsan","111");
				
				System.out.println("----"+token.getUsername());
				System.out.println("----"+token.getPrincipal());
				System.out.println("----"+token.getPassword());
				try {
					// 执行认证提交
					subject.login(token);
				} catch (AuthenticationException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
				// 是否认证通过
				boolean isAuthenticated = subject.isAuthenticated();
				System.out.println("是否认证通过：" + isAuthenticated);
				// 退出操作
				subject.logout();
				// 是否认证通过
				isAuthenticated = subject.isAuthenticated();
				System.out.println("是否认证通过：" + isAuthenticated);
	}
}
```

**AuthenticationException** 父异常

**DisabledAccountException** 账户失效异常

**ExcessiveAttemptsException** 尝试次数过多

**UnknownAccountException** 用户名不正确

**ExpiredCredentialsException** 用户名密码过期需要修改

**IncorrectCredentialsException** 密码错误

#### JDBC Realm

```xml
<dependency> 
     <groupId>com.mchange</groupId> 
     <artifactId>c3p0</artifactId> 
     <version>0.9.5.2</version> 
</dependency> 
<dependency> 
     <groupId>mysql</groupId> 
     <artifactId>mysql-connector-java</artifactId> 
     <version>5.1.47</version> 
</dependency> 
<dependency> 
     <groupId>commons-logging</groupId> 
     <artifactId>commons-logging</artifactId> 
     <version>1.2</version> 
</dependency>
```

 **配置文件方式：** 

```ini
[main]
#加入c3p0连接池
dataSource=com.mchange.v2.c3p0.ComboPooledDataSource
dataSource.driverClass=com.mysql.jdbc.Driver
dataSource.jdbcUrl=jdbc:mysql://127.0.0.1:13306/testDB?useSSL=false
dataSource.user=test
dataSource.password=test1

jdbcRealm=org.apache.shiro.realm.jdbc.JdbcRealm
jdbcRealm.dataSource=$dataSource
jdbcRealm.authenticationQuery=select password FROM ntkd_user WHERE name=?
securityManager.realm=$jdbcRealm 
```

```java
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.config.IniSecurityManagerFactory;
import org.apache.shiro.mgt.SecurityManager;
import org.apache.shiro.subject.Subject;
import org.apache.shiro.util.Factory;

public class Test {

	public static void main(String[] args) {
		// 创建securityManager工厂，通过ini配置文件创建securityManager工厂
		Factory<SecurityManager> factory = new IniSecurityManagerFactory("classpath:shiro-jdbc.ini");
		// 创建SecurityManager
		SecurityManager securityManager = factory.getInstance();
		// 将securityManager设置当前的运行环境中
		SecurityUtils.setSecurityManager(securityManager);
		// 从SecurityUtils里边创建一个subject
		Subject subject = SecurityUtils.getSubject();
		// 在认证提交前准备token（令牌）
		// 这里的账号和密码 将来是由用户输入进去
		UsernamePasswordToken token = new UsernamePasswordToken("aaa", "123");
		try {
			// 执行认证提交
			subject.login(token);
		} catch (AuthenticationException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		// 是否认证通过
		boolean isAuthenticated = subject.isAuthenticated();
		System.out.println("是否认证通过：" + isAuthenticated);
	}
}
```

 **代码方式：**  

```java
import java.beans.PropertyVetoException;
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.mgt.DefaultSecurityManager;
import org.apache.shiro.mgt.SecurityManager;
import org.apache.shiro.realm.jdbc.JdbcRealm;
import org.apache.shiro.subject.Subject;
import com.mchange.v2.c3p0.ComboPooledDataSource;

public class TestAllInClass {
	
	public static void main(String[] args) throws PropertyVetoException {
		
		ComboPooledDataSource cpds = new ComboPooledDataSource();
		cpds.setDriverClass("com.mysql.jdbc.Driver");
		cpds.setJdbcUrl("jdbc:mysql://127.0.0.1:13306/testDB?useSSL=false");
		cpds.setUser("test");
		cpds.setPassword("test1");
		
		JdbcRealm realm = new JdbcRealm();
        realm.setDataSource(cpds);
        realm.setAuthenticationQuery("select password FROM ntkd_user WHERE name=?");
        
		SecurityManager securityManager=new DefaultSecurityManager(realm);
		SecurityUtils.setSecurityManager(securityManager);
        
        Subject subject = SecurityUtils.getSubject();
        UsernamePasswordToken token = new UsernamePasswordToken("aaa", "123");
        try {
			subject.login(token);
		} catch (AuthenticationException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		boolean isAuthenticated = subject.isAuthenticated();
		System.out.println("是否认证通过：" + isAuthenticated);
	}
}
```

#### Authentication Strategy

**FirstSuccessfulStrategy**：只要有一个Realm验证成功即可，只返回第一个Realm身份验证成功的认证信息，其他的忽略；

**AtLeastOneSuccessfulStrategy**：只要有一个Realm验证成功即可，和FirstSuccessfulStrategy不同，返回所有Realm身份验证成功的认证信息；

**AllSuccessfulStrategy**：所有Realm验证成功才算成功，且返回所有Realm身份验证成功的认证信息，如果有一个失败就失败了。

```java
<bean id="myAuthenticator" class="org.apache.shiro.authc.pam.ModularRealmAuthenticator">
	<property name="authenticationStrategy">
		<bean class="org.apache.shiro.authc.pam.AtLeastOneSuccessfulStrategy" />
	</property>
</bean> 
```

**shiro密码比对**是通过

 `AuthenticatingRealm.CredentialsMatcher.doCredentialsMatch(AuthenticationToken token, AuthenticationInfo info)`

完成的

#### realm指定计算摘要方式

shiro计算摘要值

```java
/**
* String algorithmName 算法名
* Object source  需要加密的内容
* Object salt 盐
* int hashIterations 加密次数
*/

String algorithmName="sha1";
Object source = "password";
ByteSource salt = ByteSource.Util.bytes("salt");
int hashIterations = 3;
		
SimpleHash sh1 = new SimpleHash(algorithmName, source ,salt,hashIterations);

System.out.println(sh1.toString());
```

 realm指定计算方式 

```java
<bean id="myRealm" class="com.ntkd.shirorealm.MyRealm">
	<property name="credentialsMatcher">
		<bean class="org.apache.shiro.authc.credential.HashedCredentialsMatcher">
			<property name="hashAlgorithmName" value="MD5" />
			<property name="hashIterations" value="3" />
		</bean>
	</property>
</bean> 
```

 realm文件 

```java
package com.ntkd.shirorealm;

import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.AuthenticationInfo;
import org.apache.shiro.authc.AuthenticationToken;
import org.apache.shiro.authc.SimpleAuthenticationInfo;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.realm.AuthenticatingRealm;
import org.apache.shiro.util.ByteSource;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

//认证值需要实现AuthenticatingRealm，重写里面的doGetAuthenticationInfo方法即可
public class MyRealm extends AuthenticatingRealm {

	private static final Logger logger = LoggerFactory.getLogger(MyRealm.class);

	@Override
	public String getName() {
		return "myRealm";
	}

	@Override
	protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {

		UsernamePasswordToken upt = (UsernamePasswordToken) token;
		logger.info("==>AuthenticationInfo.token:username:{}-password;{}", upt.getUsername(), upt.getPassword());
		
		String username = upt.getUsername();
		String password = "";
		ByteSource bsSalt = ByteSource.Util.bytes(username);
		
		//md5
		if ("aaa".equals(username)) {
			//密码111
			password = "c2d407174963907e7f85ca84e05c874f";
		}
		if ("bbb".equals(username)) {
			//密码222
			password = "7fdc4b70c261986b3b14cc641d5d6024";
		}
		SimpleAuthenticationInfo info = new SimpleAuthenticationInfo(username, password,bsSalt, getName());
		return info;
	}
}
```

#### Authorizor(授权)

```java
package com.ntkd.shirorealm;

import java.util.HashSet;
import java.util.Set;

import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.AuthenticationInfo;
import org.apache.shiro.authc.AuthenticationToken;
import org.apache.shiro.authc.SimpleAuthenticationInfo;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.authz.SimpleAuthorizationInfo;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;
import org.apache.shiro.util.ByteSource;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;


/**
 * 本来继承的AuthenticatingRealm类，只重写认证方法
 * 现在需要验证授权，我们需要集成他的子类AuthorizingRealm
 * 认证方法还在，多了授权方法
 */

public class MyRealm extends AuthorizingRealm {
	
	private static final Logger logger = LoggerFactory.getLogger(MyRealm.class);

	@Override
	public String getName() {
		// TODO Auto-generated method stub
		return "myRealm";
	}

	@Override
	protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
		// logger.info("==>AuthenticationToken:{}",token);

		UsernamePasswordToken upt = (UsernamePasswordToken) token;
		
		logger.info("==>AuthenticationInfo.token:username:{}-password;{}", upt.getUsername(), upt.getPassword());
		
		String username = upt.getUsername();
		String password = "";
		ByteSource bsSalt = ByteSource.Util.bytes(username);
		
		//md5
		if ("aaa".equals(username)) {
			//密码111
			password = "c2d407174963907e7f85ca84e05c874f";
		}
		if ("bbb".equals(username)) {
			//密码222
			password = "7fdc4b70c261986b3b14cc641d5d6024";
		}

		SimpleAuthenticationInfo info = new SimpleAuthenticationInfo(username, password,bsSalt, getName());

		return info;
	}

	@Override
	protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
		logger.info("==>doGetAuthorizationInfo");
		for(Object o :principals )
		{
			logger.info("==>principals:{}",o);	
		}
		//获取用户登录信息
		Object principal = principals.getPrimaryPrincipal();
		
		//设定登录就有user角色，如果是aaa还有admin角色
		Set<String> roles = new HashSet<String>();
		roles.add("user");
		
		if("aaa".equals(principal)) {
			roles.add("admin");
		}
		
		SimpleAuthorizationInfo info = new SimpleAuthorizationInfo(roles);
		return info;
	}
}
```

```xml
<!-- 6. 配置 ShiroFilter. 6.1 id 必须和 web.xml 文件中配置的 DelegatingFilterProxy 
		的 <filter-name> 一致. 若不一致, 则会抛出: NoSuchBeanDefinitionException. 因为 Shiro 会来 
		IOC 容器中查找和 <filter-name> 名字对应的 filter bean. -->
	<bean id="shiroFilter"
		class="org.apache.shiro.spring.web.ShiroFilterFactoryBean">
		<property name="securityManager" ref="mySecurityManager" />
		<property name="loginUrl" value="/login.jsp" />
		<property name="successUrl" value="/main.jsp" />
		<property name="unauthorizedUrl" value="/unauthorized.jsp" />
		<property name="filterChainDefinitions">
			<value>
				/login.jsp = anon
				/shiro/login = anon
				/shiro/logout = logout
				<!-- 设置访问url需要的角色 -->
				/admin.jsp = roles[admin]
				/user.jsp = roles[user]
				
				/** = authc
			</value>
		</property>
	</bean> 
```

#### spring 整合shiro

**添加依赖**

```xml
<properties>
		<spring-version>4.3.20.RELEASE</spring-version>
	</properties>

	<dependencies>
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>javax.servlet-api</artifactId>
			<version>3.1.0</version>
		</dependency>
		<dependency>
			<groupId>javax.servlet.jsp</groupId>
			<artifactId>javax.servlet.jsp-api</artifactId>
			<version>2.3.1</version>
		</dependency>

		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>5.1.47</version>
		</dependency>

		<!-- https://mvnrepository.com/artifact/org.slf4j/slf4j-log4j12 -->
		<!-- artifactId说明 slf4j-log4j12:使用slf4j和log4j组合 slf4j-jdk14：使用slf4j和jdk日志组合 -->

		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-log4j12</artifactId>
			<version>1.7.25</version>
		</dependency>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>3.8.1</version>
		</dependency>

		<!--Spring -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-core</artifactId>
			<version>${spring-version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-aop</artifactId>
			<version>${spring-version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-orm</artifactId>
			<version>${spring-version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-web</artifactId>
			<version>${spring-version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>${spring-version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-tx</artifactId>
			<version>${spring-version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-test</artifactId>
			<version>${spring-version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-jdbc</artifactId>
			<version>${spring-version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-mock</artifactId>
			<version>2.0.8</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>${spring-version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-expression</artifactId>
			<version>${spring-version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context-support</artifactId>
			<version>${spring-version}</version>
		</dependency>

		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjweaver</artifactId>
			<version>1.8.13</version>
		</dependency>

		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis</artifactId>
			<version>3.4.6</version>
		</dependency>

		<!-- 对应的版本去http://www.mybatis.org/spring/看 -->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis-spring</artifactId>
			<version>1.3.2</version>
		</dependency>

		<dependency>
			<groupId>com.fasterxml.jackson.core</groupId>
			<artifactId>jackson-core</artifactId>
			<version>2.9.5</version>
		</dependency>
		<dependency>
			<groupId>com.fasterxml.jackson.core</groupId>
			<artifactId>jackson-databind</artifactId>
			<version>2.9.5</version>
		</dependency>
		<dependency>
			<groupId>com.fasterxml.jackson.core</groupId>
			<artifactId>jackson-annotations</artifactId>
			<version>2.9.5</version>
		</dependency>

		<dependency>
			<groupId>commons-io</groupId>
			<artifactId>commons-io</artifactId>
			<version>2.4</version>
		</dependency>
		<dependency>
			<groupId>commons-fileupload</groupId>
			<artifactId>commons-fileupload</artifactId>
			<version>1.3.1</version>
		</dependency>

		<dependency>
			<groupId>org.apache.shiro</groupId>
			<artifactId>shiro-core</artifactId>
			<version>1.3.2</version>
		</dependency>
		<dependency>
			<groupId>org.apache.shiro</groupId>
			<artifactId>shiro-spring</artifactId>
			<version>1.3.2</version>
		</dependency>
		<dependency>
			<groupId>org.apache.shiro</groupId>
			<artifactId>shiro-ehcache</artifactId>
			<version>1.3.2</version>
		</dependency>
	</dependencies>
```

```
<!-- spring -->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath:applicationContext.xml</param-value>
	</context-param>
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>


	<!-- springmvc -->
	<servlet>
		<servlet-name>springmvc</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<!-- 加载springmvc配置 -->
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<!-- 配置文件的地址 如果不配置contextConfigLocation， 默认查找的配置文件名称classpath下的：servlet名称+"-serlvet.xml"即：springmvc-serlvet.xml -->
			<param-value>classpath:springmvc.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>springmvc</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>

	<filter>
		<filter-name>encoding</filter-name>
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
		<init-param>
			<param-name>encoding</param-name>
			<param-value>utf-8</param-value>
		</init-param>
	</filter>
	<filter-mapping>
		<filter-name>encoding</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>
	
	<!-- 
	1. 配置  Shiro 的 shiroFilter.  
	2. DelegatingFilterProxy 实际上是 Filter 的一个代理对象. 默认情况下, Spring 会到 IOC 容器中查找和 
	<filter-name> 对应的 filter bean. 也可以通过 targetBeanName 的初始化参数来配置 filter bean 的 id. 
	-->
	<!-- shiro -->
    <filter>
        <filter-name>shiroFilter</filter-name>
        <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
        <init-param>
            <param-name>targetFilterLifecycle</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>

    <filter-mapping>
        <filter-name>shiroFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping> 
```

**web.xml**

```xml
<!-- spring -->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath:applicationContext.xml</param-value>
	</context-param>
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>


	<!-- springmvc -->
	<servlet>
		<servlet-name>springmvc</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<!-- 加载springmvc配置 -->
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<!-- 配置文件的地址 如果不配置contextConfigLocation， 默认查找的配置文件名称classpath下的：servlet名称+"-serlvet.xml"即：springmvc-serlvet.xml -->
			<param-value>classpath:springmvc.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>springmvc</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>

	<filter>
		<filter-name>encoding</filter-name>
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
		<init-param>
			<param-name>encoding</param-name>
			<param-value>utf-8</param-value>
		</init-param>
	</filter>
	<filter-mapping>
		<filter-name>encoding</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>
	
	<!-- 
	1. 配置  Shiro 的 shiroFilter.  
	2. DelegatingFilterProxy 实际上是 Filter 的一个代理对象. 默认情况下, Spring 会到 IOC 容器中查找和 
	<filter-name> 对应的 filter bean. 也可以通过 targetBeanName 的初始化参数来配置 filter bean 的 id. 
	-->
	<!-- shiro -->
    <filter>
        <filter-name>shiroFilter</filter-name>
        <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
        <init-param>
            <param-name>targetFilterLifecycle</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>

    <filter-mapping>
        <filter-name>shiroFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping> 
```

**applicationContext.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="                                             
            http://www.springframework.org/schema/beans  
            http://www.springframework.org/schema/beans/spring-beans.xsd  
            http://www.springframework.org/schema/context   
            http://www.springframework.org/schema/context/spring-context.xsd  
            http://www.springframework.org/schema/mvc  
            http://www.springframework.org/schema/mvc/spring-mvc.xsd
            http://www.springframework.org/schema/tx 
            http://www.springframework.org/schema/tx/spring-tx.xsd
            http://www.springframework.org/schema/aop
            http://www.springframework.org/schema/aop/spring-aop.xsd ">

    <!--  缓存配置，暂时不管 -->
	<bean id="myCacheManager"
		class="org.apache.shiro.cache.ehcache.EhCacheManager">
		<property name="cacheManagerConfigFile"
			value="classpath:ehcache.xml" />
	</bean>

    <!--  
        定义.ModularRealmAuthenticator 可以指定认证的匹配规则authenticationStrategy
        默认是多个realm里有一个成功就行。AtLeastOneSuccessfulStrategy
    -->
	<bean id="myAuthenticator"
		class="org.apache.shiro.authc.pam.ModularRealmAuthenticator">
		<property name="authenticationStrategy">
			<bean class="org.apache.shiro.authc.pam.AtLeastOneSuccessfulStrategy"></bean>
		</property>
<!-- 		<property name="realms">
			<list>
    			<ref bean="myRealm"/>
  				<ref bean="myRealm2"/>
    		</list>
		</property> -->
	</bean>

    <!--  
        自定义realm,通过credentialsMatcher属性指定加密算法和方式 
    -->
	<bean id="myRealm" class="com.ntkd.shirorealm.MyRealm">
		<property name="credentialsMatcher">
			<bean class="org.apache.shiro.authc.credential.HashedCredentialsMatcher">
				<property name="hashAlgorithmName" value="MD5" />
				<property name="hashIterations" value="3" />
			</bean>
		</property>
	</bean>
	<bean id="myRealm2" class="com.ntkd.shirorealm.MyRealm2">
		<property name="credentialsMatcher">
			<bean class="org.apache.shiro.authc.credential.HashedCredentialsMatcher">
				<property name="hashAlgorithmName" value="SHA1" />
				<property name="hashIterations" value="3" />
			</bean>
		</property>
	</bean>

    <!-- 这里也就是SecurityManager -->
	<bean id="mySecurityManager"
		class="org.apache.shiro.web.mgt.DefaultWebSecurityManager">
		<property name="cacheManager" ref="myCacheManager" />
		<property name="authenticator" ref="myAuthenticator"/>
		<property name="realms">
        	<list>
    			<ref bean="myRealm"/>
  				<ref bean="myRealm2"/>
    		</list>
        </property>
	</bean>
    
    <!-- 由spring来管理生命周期 -->
	<bean id="lifecycleBeanPostProcessor"
		class="org.apache.shiro.spring.LifecycleBeanPostProcessor" />
	<!-- 5. 启用 IOC 容器中使用 shiro 的注解. 但必须在配置了 LifecycleBeanPostProcessor 之后才可以使用. -->
	<bean
		class="org.springframework.aop.framework.autoproxy.DefaultAdvisorAutoProxyCreator"
		depends-on="lifecycleBeanPostProcessor" />
	<bean
		class="org.apache.shiro.spring.security.interceptor.AuthorizationAttributeSourceAdvisor">
		<property name="securityManager" ref="mySecurityManager" />
	</bean>

	<!-- 6. 配置 ShiroFilter. 6.1 id 必须和 web.xml 文件中配置的 DelegatingFilterProxy 
		的 <filter-name> 一致. 若不一致, 则会抛出: NoSuchBeanDefinitionException. 因为 Shiro 会来 
		IOC 容器中查找和 <filter-name> 名字对应的 filter bean. -->
	<bean id="shiroFilter"
		class="org.apache.shiro.spring.web.ShiroFilterFactoryBean">
		<property name="securityManager" ref="mySecurityManager" />
		<property name="loginUrl" value="/login.jsp" />
		<property name="successUrl" value="/main.jsp" />
		<property name="unauthorizedUrl" value="/404.jsp" />
		<property name="filterChainDefinitions">
			<value>
				/login.jsp = anon
				/shiro/login = anon
				/shiro/logout = logout
				/** = authc
			</value>
		</property>
	</bean>
</beans>
```

**springmvc.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:p="http://www.springframework.org/schema/p"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xmlns:task="http://www.springframework.org/schema/task"
	xsi:schemaLocation="
        http://www.springframework.org/schema/beans 
        http://www.springframework.org/schema/beans/spring-beans-4.2.xsd 
        http://www.springframework.org/schema/context 
        http://www.springframework.org/schema/context/spring-context-4.2.xsd 
        http://www.springframework.org/schema/mvc 
        http://www.springframework.org/schema/mvc/spring-mvc-4.2.xsd 
        http://www.springframework.org/schema/task 
        http://www.springframework.org/schema/task/spring-task-4.2.xsd">

	<!-- 直接扫描包就行了，加载下面的controller -->
	<context:component-scan base-package="com.ntkd.controller"/>

	<mvc:annotation-driven/> 

	<!-- 视图解析器 -->
	<!-- 默认可以解析jstl标签 -->
	<bean
		class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<!-- 视图解析器的前缀和后缀，这样在setview可以少写一点路径 -->
		<property name="prefix" value="/" />
		<property name="suffix" value=".jsp" />
	</bean>
	
</beans>
```

**ehcache.xml**

```xml
<ehcache>
    <diskStore path="java.io.tmpdir"/>
    
    <cache name="authorizationCache"
           eternal="false"
           timeToIdleSeconds="3600"
           timeToLiveSeconds="0"
           overflowToDisk="false"
           maxBytesLocalHeap="100M"
           statistics="true">
    </cache>

    <cache name="authenticationCache"
           eternal="false"
           timeToIdleSeconds="3600"
           timeToLiveSeconds="0"
           overflowToDisk="false"
           maxBytesLocalHeap="100M"
           statistics="true">
    </cache>

    <cache name="shiro-activeSessionCache"
           eternal="false"
           timeToIdleSeconds="3600"
           timeToLiveSeconds="0"
           overflowToDisk="false"
           maxBytesLocalHeap="100M"
           statistics="true">
    </cache>

    <!--Default Cache configuration. These will applied to caches programmatically created through
        the CacheManager.

        The following attributes are required for defaultCache:

        maxInMemory       - Sets the maximum number of objects that will be created in memory
        eternal           - Sets whether elements are eternal. If eternal,  timeouts are ignored and the element
                            is never expired.
        timeToIdleSeconds - Sets the time to idle for an element before it expires. Is only used
                            if the element is not eternal. Idle time is now - last accessed time
        timeToLiveSeconds - Sets the time to live for an element before it expires. Is only used
                            if the element is not eternal. TTL is now - creation time
        overflowToDisk    - Sets whether elements can overflow to disk when the in-memory cache
                            has reached the maxInMemory limit.

        -->
    <defaultCache
        maxElementsInMemory="10000"
        eternal="false"
        timeToIdleSeconds="120"
        timeToLiveSeconds="120"
        maxBytesLocalHeap="100M"
        overflowToDisk="true"
        />

</ehcache>
```

**log4j.properties**

```ini
log4j.rootLogger=info, stdout

log4j.appender.stdout=org.apache.log4j.ConsoleAppender 
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout 
#log4j.appender.stdout.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss,SSS} %p [%l] - %m%n 
log4j.appender.stdout.layout.ConversionPattern=%m%n 

```

