#### 1.配置bean

- xml配置

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import com.ntkd.bean.Person;

// @Configuration 
// 定义配置类，相当于配置文件
@Configuration 
public class MainConfig {

	@Bean
	public Person person() {
		return new Person("张三", 14);
	}
    
}
```

- 基于注解

```java
//获取容器
//使用AnnotationConfigApplicationContext获取
//指定配置类
AnnotationConfigApplicationContext applicationContext = new AnnotationConfigApplicationContext(
				MainConfig.class);

//显示容器内组件
String[] names = applicationContext.getBeanDefinitionNames();
for (String name : names)
    System.out.println(name);

//获取person
Person person = applicationContext.getBean(Person.class);
System.out.println(person);

//关闭容器
applicationContext.close();
```

#### 2.配置component-scan

- XML

```xml
<!-- 类标注了@Controller、@Service、@Repository、@Component
@Controller标注在controller
@Service标注在service
@Repository标注在dao
@Component是
 都会被扫描加载进来-->
<context:component-scan base-package="com.ntkd.service"/>
```

- 注解

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import com.ntkd.bean.Person;

//扫描组件
//配置bao扫描，通过使用excludeFilters = @Filter[]排除,使用includeFilters = @Filter[]包含
//FilterType.ANNOTATION 按照注解过滤@Filter(type = FilterType.ANNOTATION,value = Controller.class) 
//FilterType.ASSIGNABLE_TYPE 按照类型过滤@Filter(type = FilterType.ASSIGNABLE_TYPE,value = UserService.class)
@ComponentScan({"com.ntkd.service"})
//排除
//				excludeFilters = {
//						@Filter(type = FilterType.ANNOTATION, 
//								value = { Controller.class, Service.class }) 
//						}
//包含,必须配置useDefaultFilters=false
//				includeFilters = {
//						@Filter(type = FilterType.ANNOTATION, 
//								value = { Controller.class, Service.class }) 
//						},useDefaultFilters=false

@Configuration 
public class MainConfig {

	@Bean
	public Person person() {
		return new Person("张三", 14);
	}
}
```

#### 3.作用域scope

```xml
<bean id="person" class="com.ntkd.bean.Person" scope="singleton">
	<property name="name" value="jack"></property>
	<property name="age" value="12"></property>
</bean>
```
```java
//	prototype 多例，容器启动不会调用方法创建对象，在实际applicationContext.getBean("person");时候才调用
//	singleton 默认单例，ioc容器启动就会调用这里的方法放到容器中
//	request
//	session
@Scope("prototype")
@Bean
public Person person() {
	return new Person("张三", 14);
}
```
#### 4.懒加载lazy

```java
//懒加载@Lazy
//对于单例 容器启动就会创建，配置懒加载，在获取的时候才第一次创建，以后就不创建了
@Lazy
@Bean
public Person person() {
	return new Person("张三", 14);
}
```

#### 5.按条件注册Conditional

```java
//条件判断
//在这个方法上，满足条件return true，就可以执行方法
//在类上，满足条件，类下面的内容才生效
//Windows.class，Linux.class实现Condition接口
@Conditional({Windows.class})
@Bean(name = "bill")
public Person person01() {
	return new Person("bill", 50);
}

@Conditional({Linux.class})
@Bean(name = "linus")
public Person person02() {
	return new Person("linus", 40);
}
```
```java
import org.springframework.context.annotation.Condition;
import org.springframework.context.annotation.ConditionContext;
import org.springframework.core.env.Environment;
import org.springframework.core.type.AnnotatedTypeMetadata;

public class Windows implements Condition {public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
			//这里context可以获取很多信息
    		//ioc的beanFactory context.getBeanFactory();
			//类加载器 context.getClassLoader();
			//bean定义类 context.getRegistry();
			//运行环境
			Environment environment = context.getEnvironment();
			String osname = environment.getProperty("os.name");
			if(osname.contains("indows"))
				return true;
    		//一个类似的类linux只有这里不同
    		//if(osname.contains("inux"))
			//	return true;
			return false;
	}
}
```

#### 6.给容器快速添加组件Import

```java

//方式1 快速增加类Animal.class
@Import({Animal.class,MyImportSelector.class,MyImportBeanDefinitionRegistrar.class})
public class MainConfig {
	//@Import 和是sring版本有关 老版（4.1.2）引入类上要加@Repository 后面的版本（4.3.25）不需要加 
    //id是组件全类名 
	//方式1 @Import(Animal.class)类，可以一次多个类@Import({Animal.class，Animal1.class})
	//方式2 @Import(MyImportSelector.class) 自己实现ImportSelector 返回要导入的组件
	//方式3 @Import(MyImportBeanDefinitionRegistrar.class)
}
```

```java
import org.springframework.context.annotation.ImportSelector;
import org.springframework.core.type.AnnotationMetadata;
//方式2 自己实现 ImportSelector 返回要导入的组件
public class MyImportSelector implements ImportSelector{
	public String[] selectImports(AnnotationMetadata importingClassMetadata) {
		return new String[]{"com.ntkd.bean.Dog","com.ntkd.bean.Cat"};
	}
}
```
```java
import org.springframework.beans.factory.support.BeanDefinitionRegistry;
import org.springframework.beans.factory.support.RootBeanDefinition;
import org.springframework.context.annotation.ImportBeanDefinitionRegistrar;
import org.springframework.core.type.AnnotationMetadata;

import com.ntkd.bean.Fox;
//方式3 实现ImportBeanDefinitionRegistrar 手工注册bean
public class MyImportBeanDefinitionRegistrar implements ImportBeanDefinitionRegistrar {
    public void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, BeanDefinitionRegistry registry) {
        // AnnotationMetadata 当前类的注解信息
        // BeanDefinitionRegistry BeanDefinition的注册类
        //写一些逻辑
        boolean cat = registry.containsBeanDefinition("com.ntkd.bean.Cat");
        boolean dog = registry.containsBeanDefinition("com.ntkd.bean.Dog");
        if(cat && dog) {
            //手工指定bean的信息，注册bean
            RootBeanDefinition rootBeanDefinition = new RootBeanDefinition(Fox.class);
            registry.registerBeanDefinition("fox", rootBeanDefinition);
        }
    }
}
```
#### 7.注册组件FactoryBean

```java
import org.springframework.beans.factory.FactoryBean;

//创建一个类实现spring的FactoryBean
//通过FactoryBean来产生bean
public class AnimalFactory implements FactoryBean<Animal>{
    //返回一个animal对象，添加到容器中
    public Animal getObject() throws Exception {
        // TODO Auto-generated method stub
        return new Animal("tttttttttttttttttt");
    }
    //对象类型
    public Class<?> getObjectType() {
        // TODO Auto-generated method stub
        return Animal.class;
    }
    //对象是否单例 true 单例 false 多例
    public boolean isSingleton() {
        // TODO Auto-generated method stub
        return true;
    }
}
```

```java
//在configuration中注册FactoryBean
// 默认得到的对象是调用FactoryBean的getObjectType()得到的对象
//	获取对象本身，获取AnimalFactor.class，得到的是Animal.class
//	Object animalFactory = applicationContext.getBean("animalFactory");
//	获取到factoryBean本身 前缀加 “&”
//	Object animalFactory1 = applicationContext.getBean("&animalFactory");

@Bean
public AnimalFactory animalFactory() {
	return new AnimalFactory();
}
```
#### 8.给容器添加bean方式总结

1. 包扫描+组件标注注解（@ComponentScan+@Controller、@Service、@Repository\@Component），仅限于自己写源码的类
2. @Bean，注入没有上面组件注解的类
3. @Import 
4. FactoryBean public class AnimalFactory implements FactoryBean<Animal>

#### 9.生命周期初始化和销毁

```java
/*
 * bean生命周期： 创建 -- 初始化 -- 销毁
 * 创建（构造函数）：
 * 	单例：容器启动时候
 * 	多例：实际调用时候
 * 初始化：
 * 	对象创建完成并赋值好了后，调用初始化方法
 * 销毁：
 * 	单例：容器关闭的时候
 * 	多例：容器不管，自己销毁
 * 
 * 容器管理bean的生命周期，我们可以自定义bean的初始化和销毁方法
 * 
 * 	方式1：通过xml中bean标签指定 init-method和destory-method
 * 			通过@Bean(initMethod = "init",destroyMethod = "destory")指定类里面自定义的初始化和销毁方法
 * 	方式2：类实现接口InitializingBean、DisposableBean的afterPropertiesSet()、destroy()两个方法
 * 		在afterPropertiesSet()中处理初始化，在destroy()中处理销毁
 * 	方式3：JSR-250 引入javax.annotation-api包，使用注解@PostConstruct和@PreDestroy
 * 		javax.annotation.PostConstruct 被容器装配完成后执行的一些方法，用在方法上
 *		javax.annotation.PreDestroy 容器销毁bean前执行的一些方法，用在方法上
 *	方式4：通过实现接口 BeanPostProcessor bean后置处理器,初始化前后工作
 *		postProcessBeforeInitialization  在init之前执行
 *		postProcessAfterInitialization   在init之后执行
 * BeanPostProcessor很重要，Spring底层很多功能都是用BeanPostProcessor来实现的
 * 
 * */
```



```java
//方式1 类中定义号了init和destory方法，方法名随意
public class Car {
    
    public Car() {
        System.out.println("car 对象创建了");
    }
    public void init() {
        System.out.println("car...init...");
    }
    public void destory() {
        System.out.println("car...destory...");
    }
}

//方式1：Configuration类中指定initMethod和destroyMethod
@Bean(initMethod = "init",destroyMethod = "destory")
public Car car() {
    return new Car();
}
//方式2：类实现接口InitializingBean、DisposableBean的afterPropertiesSet()、destroy()两个方法
public Car2 car2() {
    return new Car2();
}
//方式3：
public Car3 car3() {
    return new Car3();
}
//方式4：
public MyBeanPostProcessor myBeanPostProcessor() {
    return new MyBeanPostProcessor();
}
```

```java
import org.springframework.beans.factory.DisposableBean;
import org.springframework.beans.factory.InitializingBean;
import org.springframework.stereotype.Component;
//方式2：类实现接口InitializingBean、DisposableBean的afterPropertiesSet()、destroy()两个方法
public class Car2 implements InitializingBean,DisposableBean{
	public Car2() {
		System.out.println("car2 对象创建了");
	}
	/*
	 * DisposableBean 销毁方法
	 */
	public void destroy() throws Exception {
		System.out.println("car2...destory...");
	}
	/*
	 * InitializingBean 初始化方法，bean创建完成，属性赋值好
	 */
	public void afterPropertiesSet() throws Exception {
		System.out.println("car2...afterPropertiesSet...");
	}
}
```

```java
import javax.annotation.PostConstruct;
import javax.annotation.PreDestroy;
//方式3：JSR-250 引入javax.annotation-api包，使用注解@PostConstruct和@PreDestroy
public class Car3 {
	public Car3() {
		System.out.println("car3 对象创建了");
	}
    //对象创建并赋值后调用
	@PostConstruct
	public void init() {
		System.out.println("car3...@PostConstruct...");
	}
    //容器销毁对象之前
	@PreDestroy
	public void destory() {
		System.out.println("car3...@PreDestroy...");
	}
}
```

```java
import org.springframework.beans.BeansException;
import org.springframework.beans.factory.config.BeanPostProcessor;
//方式4：通过实现接口 BeanPostProcessor bean后置处理器,初始化前后工作
public class MyBeanPostProcessor implements BeanPostProcessor{

	public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
		System.out.println(beanName+"...方式4：实现接口BeanPostProcessor的postProcessBeforeInitialization()方法..."+bean);
		return bean;
	}

	public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
		System.out.println(beanName+"...方式4：实现接口BeanPostProcessor的postProcessAfterInitialization()方法..."+bean);
		return bean;
	}
}
```

```java
//创建过程
//Carall...构造方法...
//carall...方式4：实现接口BeanPostProcessor的postProcessBeforeInitialization()方法...com.ntkd.bean.Carall@4d49af10
//Carall...方式3：JSR250-@PostConstruct...
//Carall...方式2：实现接口InitializingBean的afterPropertiesSet()方法...
//Carall...方式1：initMethod=init22()...
//carall...方式4：实现接口BeanPostProcessor的postProcessAfterInitialization()方法...com.ntkd.bean.Carall@4d49af10

//销毁过程
//Carall...方式3：JSR250-@PreDestroy...
//Carall...方式2：实现接口DisposableBean的destroy()方法...
//Carall...方式1：destroyMethod=destory22()...
//容器关闭
```

#### 10.属性赋值@Value和导入配置文件@PropertySource

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.PropertySource;

import com.ntkd.bean.Person2;

//加载外部配置文件,这个配置文件的数据保存到envirement中
//可以通过下面获取到值
//ConfigurableEnvironment environment = applicationContext.getEnvironment();
//System.out.println(environment.getProperty("person.nickName"));

/*
 * spring 获取 properties的三种方式
 * @Value 属性
 * @Value 方法参数
 * 通过得到EmbeddedValueResolverAware得到StringValueResolver来解析
*/
@PropertySource(value = {"classpath:person.properties"},encoding = "utf-8")
@Configuration
public class MainConfigPropertyValue {
	
	@Bean
	public Person2 person2() {
		return new Person2();
	}
}
```

```java
package com.ntkd.bean;

import org.springframework.beans.factory.annotation.Value;

public class Person2 {
	
	//使用@Value对类赋值
	//value内容可以是
	//	1.基本数据
	//	2.SpEL #{}
	//	3.${} 取配置文件中的值，最后放到运行环境变量中
	
	@Value("张三")
	private String name;
	@Value("#{20-3}")
	private Integer age;
	@Value("${person.nickName}")
	private String nickName;
	
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public Integer getAge() {
		return age;
	}
	public void setAge(Integer age) {
		this.age = age;
	}
	public String getNickName() {
		return nickName;
	}
	public void setNickName(String nickName) {
		this.nickName = nickName;
	}
	public Person2(String name, Integer age, String nickName) {
		super();
		this.name = name;
		this.age = age;
		this.nickName = nickName;
	}
	public Person2() {
		super();
	}
	@Override
	public String toString() {
		return "Person2 [name=" + name + ", age=" + age + ", nickName=" + nickName + "]";
	}
}

```

#### 11.自动注入

spring利用依赖注入（DI），完成对容器中各个组件的赋值

注入方式有两种

```java
@Configuration
@ComponentScan({"com.ntkd.service","com.ntkd.dao"})
public class MainConfigAutowired {

}
```

```java
import org.springframework.stereotype.Repository;

@Repository
public class UserDao {

}
```

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.ntkd.dao.UserDao;

@Service
public class UserService {
	
    //方式1：@Autowired spring定义
 	//先按类型去找，applicationContext.getBean(UserService.class)
 	//如果有多个，再按beanid去找 applicationContext.getBean("userService");
 	//或者使用@Qualifer("userService")指定要加载的beanid
     //或者给多个相同类型的Bean上的一个加上@Primary,让这个成为首选，但是最终还可以用@Qualifer指定
     //默认一定要找到，找不到就报错，使用@Autowired(required=false)	可以忽略找不到
	@Primary	//多个相同类型，指定为首选注入
    @Qualifer("userService")	//指定id注入
    @Autowired	//自动注入
	private UserDao userDao;
	
	public void print() {
		System.out.println(userDao);
	}
/*
 *  @Autowired位置  CONSTRUCTOR构造器, METHOD方法, PARAMETER参数, FIELD属性, ANNOTATION_TYPE
 * 1.标注在FIELD属性上，我们常用的
 * 	@Autowired
 * 	private Car car;
 
 * 2.标注在METHOD方法上，方法参数会从容器中获取
 * 	@Autowired
 * 	public setCar(Car car){
 * 		this.car = car;
 * 	}
 
 * 3.标注在CONSTRUCTOR构造器上，有参，参数会从容器中获取,如果类只有一个有参构造器，@Autowired可以省略，啥也不用写
 *  @Autowired
 * 	public Boss (Car car){
 * 		this.car = car;
 * 	}
 * 
 *  可以省略
 * 	public Boss (Car car){
 * 		this.car = car;
 * 	}
 * 
 * 4.标注在PARAMETER参数上，参数会从容器中获取
 * 	public Boss (@Autowired Car car){
 * 		this.car = car;
 * 	}
 */

}
```

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.ntkd.dao.UserDao;

@Service
public class UserService {
	
	//方式2：@Resource(jsr-250)(avax.annotation-api)和@Inject(jsr-330)(javax.inject) 
 	//都需要导入不同的jar包
 	//@Resource 
 	//	默认先按beanid去找，@Resource(name="userService3")指定beanid
 	//	不支持@Qualifer，@Primary
 	//@Inject 
 	//	和@Autowired一样，但是没有require属性
    @Resource(name="userService")	//指定id注入
	private UserDao userDao;
	
	public void print() {
		System.out.println(userDao);
	}

}
```

#### 12.xxxAware接口

类实现xxxAware接口，实现里面的方法，方法中我们可以得到xxx实例对象。

类implements xxxAware ，就会调用xxxAwareProcessor，组件创建的时候，会得到Spring的底层组件

```java
@Bean
public MyApplicationContext myApplicationContext() {
	return new MyApplicationContext();
}
```

```java
import org.springframework.beans.BeansException;
import org.springframework.context.ApplicationContext;
import org.springframework.context.ApplicationContextAware;

public class MyApplicationContext implements ApplicationContextAware{

	private ApplicationContext applicationContext;
	
    //重写ApplicationContextAware里面的方法，然后我们就得到了ApplicationContext
	public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
		this.applicationContext= applicationContext;
		System.out.println("获取到的applicationContext："+this.applicationContext);
	}

	public ApplicationContext getApplicationContext() {
		return applicationContext;
	}
}
```

#### 13.环境切换@Profile

```java
import java.beans.PropertyVetoException;

import javax.sql.DataSource;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.EmbeddedValueResolverAware;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Profile;
import org.springframework.context.annotation.PropertySource;
import org.springframework.util.StringValueResolver;

import com.mchange.v2.c3p0.ComboPooledDataSource;

/*
 * @Profile
 * 根据当前环境，动态激活
 * 默认是@Profile("default")
 * 可以写在方法或者配置类上
 * 没有标注的，任何环境都加载
 * 
 * 如何切换
 * 方式1：使用启动参数-Dspring.profiles.active=dev 
 * 	run as -> run configuration -> Arguments ->VM Arguments
 * 方式2：创建AnnotationConfigApplicationContext()无参构造器，然后设置激活的选项，再注册
 * 相当于自己做了有参构造器的事情
 * AnnotationConfigApplicationContext applicationContext = new AnnotationConfigApplicationContext();
 * applicationContext.getEnvironment().setActiveProfiles("dev");
 * applicationContext.register(MainConfigProfile.class);
 * applicationContext.refresh();
 */
@Profile("dev")
@PropertySource(value= {"classpath:mysql.properties"},encoding = "UTF-8")
@Configuration
public class MainConfigProfile {
    
	@Value("${db.user}")
	private String mysqlUser;
    
    @Value("${db.password}")
	private String mysqlPassword;
    
    @Value("${db.driverClass}")
	private String mysqlDriverClass;
	
	@Profile("dev")
	@Bean
	public DataSource dataSourceDev() throws PropertyVetoException {
		ComboPooledDataSource comboPooledDataSource = new ComboPooledDataSource();
		comboPooledDataSource.setUser(mysqlUser);
		comboPooledDataSource.setPassword(mysqlPassword);
         comboPooledDataSource.setDriverClass(driverClass);
		comboPooledDataSource.setJdbcUrl("jdbc:mysql://192.168.14.131:13306/quartz");
		return comboPooledDataSource;
	}
	
	@Profile("prod")
	@Bean
	public DataSource dataSourceProd() throws PropertyVetoException {
		ComboPooledDataSource comboPooledDataSource = new ComboPooledDataSource();
		comboPooledDataSource.setUser(mysqlUser);
		comboPooledDataSource.setPassword(mysqlPassword);
         comboPooledDataSource.setDriverClass(driverClass);
		comboPooledDataSource.setJdbcUrl("jdbc:mysql://192.168.14.131:13306/permission");
		return comboPooledDataSource;
	}
}
```

#### 14.面向切面编程AOP（Aspect Oriented Programming）

```java
//业务类
//没有任何耦合代码
public class MathCalculator {
	
	public int div(int i,int j) {
		//如果日志记录在这 耦合了
		return i/j;
	}
}
```

```java
import java.util.Arrays;
import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.AfterReturning;
import org.aspectj.lang.annotation.AfterThrowing;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;

//切面类
//@Aspect表明这是切面类
@Aspect
public class MathLogAspects {
	
	//抽取公共切入点
	@Pointcut("execution(public int com.ntkd.aop.MathCalculator.*(..))")
	public void pointCut() {
		
	}
	
	//@Before("public int com.ntkd.aop.MathCalculator.div(int, int)")	//div方法
	//@Before("public int com.ntkd.aop.MathCalculator.*(..)")	//任意方法，任意参数 抽取到公共
	@Before("pointCut()") //在目标方法运行之前运行的方法--肯定执行
	public void logBegin(JoinPoint joinPoint) {
		Object[] args = joinPoint.getArgs();
		System.out.println(joinPoint.getSignature().getName()+"...@Before...参数是{"+Arrays.asList(args)+"}");
	}
	
	//@Before("public int com.ntkd.aop.MathCalculator.*(..)")	//抽取到公共
	@After("pointCut()")  //在目标方法运行结束后运行的方法--肯定执行
	public void logEnd(JoinPoint joinPoint) {
		System.out.println(joinPoint.getSignature().getName()+"...@After...");
	}
	
    //在目标方法正常运行返回结果后运行--正常执行
	@AfterReturning(value="pointCut()",returning = "result")
	public void logSuccess(JoinPoint joinPoint,Object result) {
		System.out.println(joinPoint.getSignature().getName()+"...@AfterReturning...结果是{"+result+"}");
	}
	
    //在目标方法出现异常后运行--异常执行
	@AfterThrowing(value="pointCut()",throwing = "exception")
	public void logException(JoinPoint joinPoint,Exception exception) {
		System.out.println(joinPoint.getSignature().getName()+"...@AfterThrowing...异常信息{"+exception+"}");
	}

}

```

```java
/*
 * AOP 动态代理
 * 	在程序运行期间动态的将某段代码插入到指定方法指定位置执行
 * 
 * 1.导入Spring AOP包 spring-aspects
 * 
 * 2.定义一个业务逻辑MathCalculator.java 在方法前后执行一些操作和记录日志
 * 3.定义一个切面类 MathLogAspects.java 切面类里面的方法加到业务逻辑方法周围
 * 	通知方式：
 * 		前置通知(@Before)：在目标方法运行之前运行的方法--肯定执行
 * 		后置通知(@After)：在目标方法运行结束后运行的方法--肯定执行
 * 		返回通知(@AfterReturning)：在目标方法正常运行返回结果后运行--正常执行
 * 		异常通知(@AfterThrowing)：在目标方法出现异常后运行--异常执行
 * 		环绕通知(@Around)：最底层，动态代理，手动推进目标方法运行
 * 4.切面类MathLogAspects.java的方法使用上面的通知注解指定何时运行
 * 	使用@Pointcut指定切入点
 * 	@Pointcut("execution(public int com.ntkd.aop.MathCalculator.*(..))")
 *	public void pointCut() {	
 *	}
 *
 * 5.给切面类MathLogAspects.java加上注解@Aspect
 * 6.将切面类和业务类都加到容器中
 * 7.在@Configuration类上添加@EnableAspectJAutoProxy 开启AOP模式
 * 
 * JoinPoint joinPoint可以获取业务类的一些信息，写在切面类的方法参数
 * 必须写在参数的第一个
 * @AfterReturning(value="pointCut()",returning = "result")
 * public void logSuccess(JoinPoint joinPoint,Object result) {
 *		System.out.println(joinPoint.getSignature().getName()+"...@AfterReturning...结果是{"+result+"}");
 *	}
 */

//开启AOP模式
@EnableAspectJAutoProxy
@Configuration
public class MainConfigAOP {
	
	@Bean
	public MathCalculator mathCalculator() {
		return new MathCalculator();
	}
	
	@Bean
	public MathLogAspects mathLogAspects() {
		return new MathLogAspects();
	}

}
```

#### 15.声明式事务

```java
package com.ntkd.tx;

import java.beans.PropertyVetoException;

import javax.sql.DataSource;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.datasource.DataSourceTransactionManager;
import org.springframework.transaction.PlatformTransactionManager;
import org.springframework.transaction.annotation.EnableTransactionManagement;

import com.mchange.v2.c3p0.ComboPooledDataSource;
import com.ntkd.util.ChineseName;

/*
 * 声明式事务
 * 
 * 环境搭建：导入数据驱动，数据源，springjdbc等
 * 配置数据源，使用jdbcTemplate（spring 工具 操作数据库）操作数据库
 * 	
 * 1.给方法标注@Transactional开启事务
 * 2.给配置类加上@EnableTransactionManagement标签 开启基于注解的事务管理  
 * 3.容器中注册PlatformTransactionManager的DataSourceTransactionManager管理dataSource
 * 
 * 
 */
@EnableTransactionManagement
@ComponentScan("com.ntkd.tx")
@Configuration
public class MainConfigTX {
	
	@Bean
	public DataSource dataSource() throws PropertyVetoException {
		ComboPooledDataSource comboPooledDataSource = new ComboPooledDataSource();
		comboPooledDataSource.setUser("ntkd");
		comboPooledDataSource.setPassword("wersdf234");
		comboPooledDataSource.setJdbcUrl("jdbc:mysql://192.168.14.131:13306/permission?useUnicode=true&characterEncoding=utf-8");
		comboPooledDataSource.setDriverClass("com.mysql.jdbc.Driver");
		return comboPooledDataSource;
	}
	//使用spring JdbcTemplate操作数据库
	@Bean
	public JdbcTemplate jdbcTemplate() throws PropertyVetoException {
		JdbcTemplate jdbcTemplate = new JdbcTemplate(dataSource());
		return jdbcTemplate;
	}
	//容器中注册PlatformTransactionManager的DataSourceTransactionManager管理dataSource
	@Bean
	public PlatformTransactionManager transactionManager() throws PropertyVetoException {
		return new DataSourceTransactionManager(dataSource());
	}

}

```

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

@Service
public class UserService {
	
	@Autowired
	private UserDao userDao;
	//@Transactional需要开启事务的方法上添加@Transactional
	@Transactional
	public void insertUser() {
		userDao.insert();
		System.out.println("插入完成...");
		int i = 10/0;
	}
}
```

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Repository;

import com.ntkd.util.ChineseName;

@Repository
public class UserDao {

    //使用spring JdbcTemplate操作数据库
	@Autowired
	private JdbcTemplate jdbcTemplate;
	
	public void insert() {
		String sql = "INSERT INTO `permission`.`sys_user` (`name`, `age`) VALUES (?, ?)";
		jdbcTemplate.update(sql, "小明",15);
	}
}

```

#### 16.Servlet3.0一些知识

```java
import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/*
 * @WebServlet("/hello")
 * 
 * 相当于以前在xml中配置
 * <servlet>
 *		<!-- 类名 -->
 *		<servlet-name>DisplayHeader</servlet-name>
 *		<!-- 所在的包 -->
 *		<servlet-class>test.DisplayHeader</servlet-class>
 *	</servlet>
 *	<servlet-mapping>
 *		<servlet-name>DisplayHeader</servlet-name>
 *		<!-- 访问的网址 -->
 *		<url-pattern>/DisplayHeader2</url-pattern>
 *	</servlet-mapping>
 *	
 *	扩展写法@WebServlet(description = "a enter for wechat", urlPatterns = { "/aaa"},loadOnStartup=1)  *
 注册filter用@WebFilter
 注册listener用@WebListener
 初始化参数@WebInitParam
 */

//

@WebServlet("/hello")
public class HelloTestServlet extends HttpServlet {
	
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		// TODO Auto-generated method stub
		resp.getWriter().write("hello...");
	}

}

```

```java
/* 
 * Shared libraries / runtimes pluggability
 * 共享库 运行时插件能力
 * ServletContainerInitializer在web容器启动时为提供给第三方组件机会做一些初始化的工作
 * 
 * setvlet容器启动时，会扫描每个jar包里面的
 * 	/META-INF/services/javax.servlet.ServletContainerInitializer这个文件
 * 文件的内容指向ServletContainerInitializer的实现类，实现类的全类名
 * 然后运行这个实现类里面的方法
 * 
 * ServletContainerInitializer的方法
 * 	onStartup(Set<Class<?>> c, ServletContext ctx)
 * 		Set<Class<?>> c，得到@HandlesTypes()里指定的这个类型下面的子类（实现类，子接口等），指定类本身得不到
 * 		ServletContext ctx，当前web应用的servletContext,一个web应用对应一个servletContext
 */

//@HandlesTypes()里指定的这个类型下面的子类（实现类，子接口等），指定类本身得不到
//比如HelloServic.class
//public abstract class HelloServiceAbstract implements HelloService {}
//public interface HelloServiceExt extends HelloService {}
//public class HelloServiceImpl implements HelloService {}

@HandlesTypes(value= {HelloService.class})
public class MyServletContainerInitializer implements ServletContainerInitializer{
	
	@Override
	public void onStartup(Set<Class<?>> c, ServletContext ctx) throws ServletException {
		System.out.println("init启动...");
        //得到@HandlesTypes()里指定的这个类型下面的子类（实现类，子接口等），指定类本身得不到
        //ServletContext ctx，当前web应用的servletContext,一个web应用对应一个servletContext
		for (Class<?> class1 : c) {
			System.out.println(class1);
		}
    }
    
/* 
 * 使用servletContext注册三大组件（servlet,filter,Listener）
 * 编码方式添加三大组件
 * 必须在项目启动的时候添加，不是任何时候拿到servletContext都能添加
 * 	1.ServletContainerInitializer 里面的ServletContext能注册
 * 	2.ServletContextListener里面的ServletContextEventgetServletContext()也能注册
 */
        //注册servlet
    	ServletRegistration.Dynamic userServlet = ctx.addServlet("userServlet", UserServlet.class);
        userServlet.addMapping("/user");
		
		//注册listener
		ctx.addListener(new UserListener());
    
    	//注册filter
		FilterRegistration.Dynamic userFilter = ctx.addFilter("userFilter", UserFilter.class);
		//配置filter映射信息
		//请求类型:FORWARD,INCLUDE,REQUEST,ASYNC,ERROR
		userFilter.addMappingForUrlPatterns(EnumSet.of(DispatcherType.REQUEST), true, "/*");
    
}
```

```java
import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;

/* 监听器
 * ServletContextListener监听项目的启动和停止
 */
public class UserListener implements ServletContextListener{

	//监听销毁
	@Override
	public void contextDestroyed(ServletContextEvent sce) {
		System.out.println("UserListener...contextDestroyed()...");
	}

	//监听启动
	@Override
	public void contextInitialized(ServletContextEvent sce) {
		System.out.println("UserListener...contextInitialized()...");
	}

}
```

```java
public class UserServlet extends HttpServlet {

    /*
    *servlet
    */
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		System.out.println("UserServlet...doGet()...");
		resp.getWriter().write("UserServlet...doGet()...");
	}
}
```

```java
import java.io.IOException;
import javax.servlet.AsyncContext;
import javax.servlet.ServletException;
import javax.servlet.ServletResponse;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * servlet 3.0 异步处理
 * 来了请求之后，主线程接受请求，然后发给异步处理线程，
 */
@WebServlet(value = "/async", asyncSupported = true)
public class HelloAsyncServlet extends HttpServlet {

	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

		System.out.println("主线程---开始---" + Thread.currentThread()+"---"+System.currentTimeMillis());
		// 1.开启异步支持 @WebServlet(value = "/async",asyncSupported = true)

		// 2.得到异步环境对象
		AsyncContext startAsync = req.startAsync();

		// 3.执行异步方法 startAsync.start(new Runnable() {})
		startAsync.start(new Runnable() {

			@Override
			public void run() {
				
				try {
					System.out.println("副线程---开始---" + Thread.currentThread()+"---"+System.currentTimeMillis());
					sayHello();

					// 4.异步处理完后调用
					startAsync.complete();

					// 5.得到响应
					ServletResponse response = startAsync.getResponse();

					// 6.响应返回数据
					response.getWriter().write("async---done---");
					
					System.out.println("副线程---结束---" + Thread.currentThread()+"---"+System.currentTimeMillis());
					
				} catch (InterruptedException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				} catch (IOException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
			}
		});
		
		System.out.println("主线程---结束---" + Thread.currentThread()+"---"+System.currentTimeMillis());
		
	}

	public void sayHello() throws InterruptedException {
		System.out.println("sayHello---开始---" + Thread.currentThread()+"---"+System.currentTimeMillis());
		Thread.sleep(3000);
		System.out.println("sayHello---结束---" + Thread.currentThread()+"---"+System.currentTimeMillis());
	}

}

```



#### 17.使用注解整合SpringMvc和Servlet3.0

web容器启动的时候，会扫描每个jar包下面的

```java
/META-INF/services/javax.servlet.ServletContainerInitializer
```

加载这个文件里面指定的启动类

`spring-web-4.3.25.RELEASE.jar`下正好有这个文件，内容是

```java
org.springframework.web.SpringServletContainerInitializer
```

查看这个源码

```java
@HandlesTypes(WebApplicationInitializer.class)
public class SpringServletContainerInitializer implements ServletContainerInitializer {
    	...
         if (!waiClass.isInterface() && !Modifier.isAbstract(waiClass.getModifiers()) &&
						WebApplicationInitializer.class.isAssignableFrom(waiClass))
         ...
}
```

启动的时候会加载`WebApplicationInitializer`的所有实现类(不是接口，不是抽象类)，并创建对象

```java
//`WebApplicationInitializer`
//	1.AbstractContextLoaderInitializer 
//		createRootApplicationContext()创建根容器
//	2.AbstractDispatcherServletInitializer
//		2.1.创建web容器createServletApplicationContext()
//		2.2.创建createDispatcherServlet()
//		2.3.将dispatchServlet加入到applicationContext中
//	3.AbstractAnnotationConfigDispatcherServletInitializer 注解方式DispatcherServletInitializer
//		3.1.创建applicationContext容器
//		3.2.创建webContext

//我们基于注解方式，那会以第三种方式，先用一个类继承AbstractAnnotationConfigDispatcherServletInitializer，然后重写里面的方法
```

```java
package com.ntkd.init;

import org.springframework.web.servlet.support.AbstractAnnotationConfigDispatcherServletInitializer;
import com.ntkd.config.SpringConfig;
import com.ntkd.config.SpringMvcConfig;
//MyWebAppInitializer继承AbstractAnnotationConfigDispatcherServletInitializer
public class MyWebAppInitializer extends AbstractAnnotationConfigDispatcherServletInitializer{

	//获取根容器的配置类 spring容器配置文件
	@Override
	protected Class<?>[] getRootConfigClasses() {
		// TODO Auto-generated method stub
		return new Class<?>[] {SpringConfig.class};
	}

	//获取web容器的配置类 spring mvc 容器配置文件
	@Override
	protected Class<?>[] getServletConfigClasses() {
		// TODO Auto-generated method stub
		return new Class<?>[] {SpringMvcConfig.class};
	}

	//获取servlet的dispatchservlet的mapping
	// /:拦截所有静态资源，不包括*.jsp
	// /*:拦截所有，包括*.jsp
	@Override
	protected String[] getServletMappings() {
		// TODO Auto-generated method stub
		return new String[] {"/"};
	}

}
```

```java
package com.ntkd.config;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.FilterType;
import org.springframework.context.annotation.ComponentScan.Filter;
import org.springframework.stereotype.Controller;

//spring配置文件，扫描所有包，除了controller
//根容器
@ComponentScan(value= {"com.ntkd"},excludeFilters = {
		@Filter(type=FilterType.ANNOTATION,classes = {Controller.class})
})
public class SpringConfig {
}
```

```java
package com.ntkd.config;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.ComponentScan.Filter;
import org.springframework.context.annotation.FilterType;
import org.springframework.stereotype.Controller;
import org.springframework.web.servlet.config.annotation.DefaultServletHandlerConfigurer;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.ViewResolverRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurerAdapter;

import com.ntkd.interceptor.MyInterceptor;

//spring mvc 配置文件，只扫描Controller
//子容器
//includeFilters 必须禁用默认规则useDefaultFilters = false，排除的不用管
@ComponentScan(value = { "com.ntkd" }, includeFilters = {
		@Filter(type = FilterType.ANNOTATION, classes = { Controller.class }) }, useDefaultFilters = false)
//开启springmvc高级功能，相当于开启了<mvc:annotation-driven />
//	定制：
//		实现接口WebMvcConfigurer 但是需要实现的方法太多
//		继承WebMvcConfigurerAdapter 复写他的方法
@EnableWebMvc
public class SpringMvcConfig extends WebMvcConfigurerAdapter {
//	定制
	// 视图解析器
	@Override
	public void configureViewResolvers(ViewResolverRegistry registry) {
		// 默认 return jsp("/WEB-INF/", ".jsp");
		// registry.jsp();
		registry.jsp("/WEB-INF/views/", ".jsp");
	}

	// 静态资源访问
	@Override
	public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
		// 相当于<mvc:default-servlet-handler/>
        //springmvc处理不了的请求，交给tomcat处理
		configurer.enable();
	}

	@Override
	public void addInterceptors(InterceptorRegistry registry) {
		// /** 任意路径任意请求
		registry.addInterceptor(new MyInterceptor()).addPathPatterns("/**");
	}
}
```

```java
package com.ntkd.interceptor;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

/**
 */
public class MyInterceptor implements HandlerInterceptor{

	//在目标方法之前执行
	@Override
	public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
			throws Exception {
		System.out.println("MyInterceptor---preHandle---");
		return true;
	}

	
	//目标方法执行正确之后执行
	@Override
	public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
			ModelAndView modelAndView) throws Exception {
		System.out.println("MyInterceptor---postHandle---");
	}

	//页面响应以后执行
	@Override
	public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex)
			throws Exception {
		System.out.println("MyInterceptor---afterCompletion---");
	}
}
```

```java
package com.ntkd.controller;

import java.util.UUID;
import java.util.concurrent.Callable;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.context.request.async.DeferredResult;
import com.ntkd.service.OrderService;

/**
 * //方式1.控制器返回Callable实现异步处理
 * 1.控制器返回Callable
 * 2.spring异步处理，将Callable提交到org.springframework.core.task.TaskExecutor处理
 * 3.dispatchServlet和filter退出，response还保持打开
 * 4.Callable返回结果，springmvc将请求请求重新给容器，重新执行一遍
 */


@Controller
public class AsyncController {

	//方式1.控制器返回Callable实现异步处理
	@ResponseBody
	@RequestMapping("/async01")
	public Callable<String> async01(){
		
		System.out.println("主线程---开始---" + Thread.currentThread()+"---"+System.currentTimeMillis());
		
		Callable<String> callable = new Callable<String>() {

			@Override
			public String call() throws Exception {
				System.out.println("副线程---开始---" + Thread.currentThread()+"---"+System.currentTimeMillis());
				Thread.sleep(2000);
				System.out.println("副线程---结束---" + Thread.currentThread()+"---"+System.currentTimeMillis());
				return "AsyncController---async01()---";
			}
		};
		System.out.println("主线程---开始---" + Thread.currentThread()+"---"+System.currentTimeMillis());
		
		return callable;
	}
	
	//方式2，通过DeferredResult实现异步处理
	@ResponseBody
	@RequestMapping("/createrOrder")
	public DeferredResult<Object> createrOrder() {
		System.out.println("createrOrder---开始---" + Thread.currentThread()+"---"+System.currentTimeMillis());
		
		//请求到达，创建DeferredResult 设定超时时间和超时返回
		DeferredResult<Object> deferredResult = new DeferredResult<Object>((long)3000,"create failed----");
		
		//将DeferredResult保存到队列Queue，一旦这个DeferredResult有值，就会返回出去
		OrderService.save(deferredResult);
		
		System.out.println("createrOrder---结束---" + Thread.currentThread()+"---"+System.currentTimeMillis());
		return deferredResult;
	}
	
	@ResponseBody
	@RequestMapping("/create")
	public String create() {
		System.out.println("create---开始---" + Thread.currentThread()+"---"+System.currentTimeMillis());

		String orderId = UUID.randomUUID().toString();
		DeferredResult<Object> deferredResult = OrderService.get();
		deferredResult.setResult(orderId);
		
		System.out.println("create---结束---" + Thread.currentThread()+"---"+System.currentTimeMillis());
		return "order id : "+orderId;
	}	
}
```

