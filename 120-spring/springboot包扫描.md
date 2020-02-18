# springboot包扫描

方法1：

Springboot Application的main类的方法和controller包处于同一级

方法2：

在Springboot Application的main方法类中添加注解

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.ComponentScan;

@SpringBootApplication
//添加扫描包的注解，扫描controller
@ComponentScan(basePackages="com.ntkd.controller")
public class SpringBootDemoApplication {

	public static void main(String[] args) {
		SpringApplication.run(SpringBootDemoApplication.class, args);
	}

}
```

