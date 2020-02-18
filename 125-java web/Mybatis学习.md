# Mybatis学习

**#{}和${}的区别**

\#{}sql语句使用？占位符，通过制定索引(#{0}、#{param1},他俩都指第一个参数)来获取参数

${}sql使用字符串拼接，默认找${内容}，内容的get方法获取值,如果写数字，就是数字。

 

**#{}获取参数**

1.#{0}，#{param1}获取第一个参数

2.参数是对象，#{属性名}

3.参数是map，#{key}

 

每个sqlsession默认不自动提交，使用sesson.commit()提交。也可以openSession(true)打开自动提交。

JDBC中executeUpdate()执行新增，删除，修改操作，返回int表示受影响的行数。

Mybatis中<insert><delete><update>标签，没有resultType，返回是int 。

 

 **mybatis使用方式** 

**方式1**

1.新建mapper文件，UserMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" 
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.ntkd.usermapper.UserMapper">
	<select id="selectAll" resultType="com.ntkd.userpojo.User">
		select * from t_user
	</select>
</mapper>
```

 2.mybatis.xml里面执行mapper标签指向步骤1里面的文件 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<settings>
		<setting name="logImpl" value="SLF4J" />
	</settings>
	<!-- default引用environment的id,当前所使用的环境 -->
	<environments default="default">
		<!-- 声明可以使用的环境 -->
		<environment id="default">
			<!-- 使用原生JDBC事务 -->
			<transactionManager type="JDBC"></transactionManager>
			<dataSource type="POOLED">
				<property name="driver" value="com.mysql.jdbc.Driver" />
				<property name="url"
					value="jdbc:mysql://192.168.253.132:13306/testDB?useSSL=false" />
				<property name="username" value="test" />
				<property name="password" value="test1" />
			</dataSource>
		</environment>
	</environments>

	<mappers>
	<!-- 使用mybatis方式1 -->
		<mapper resource="com/ntkd/usermapper/UserMapper.xml" />

	</mappers>
</configuration>
```

 3.java代码 

```java
public static void main(String[] args) throws IOException {
		//加载mybatis,xml配置文件
		InputStream is = Resources.getResourceAsStream("mybatis.xml");
		SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(is);
		SqlSession session = factory.openSession();
		//调用方法,找到命名空间和id，此处命名空间和id可以随意设置
		List<User> list = session.selectList("com.ntkd.usermapper.UserMapper.selectAll");
		for(User u : list)
			System.out.println(u);
		
		session.close();
	}
```

**方式2**

1.新建包：com.ntkd.usermapper

2.包下面新建mapper文件，UserMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!-- 此处命名空间必须设置为mapper接口的全路径  -->
<mapper namespace="com.ntkd.usermapper.UserMapper">
	<select id="selectAll" resultType="com.ntkd.userpojo.User" >
		select * from t_user
	</select>
</mapper> 
```

 3.包下新建接口：UserMapper 

```java
package com.ntkd.usermapper;
import java.util.List;
import com.ntkd.userpojo.User;
//接口名和mapper文件名一致
public interface UserMapper {
	//方法名和mapper里面的方法名一致
	List<User> selectAll();
} 
```

 4.mybatis.xml里面执行mapper标签指向步骤1里面的文件 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<settings>
		<setting name="logImpl" value="SLF4J" />
	</settings>
	<!-- default引用environment的id,当前所使用的环境 -->
	<environments default="default">
		<!-- 声明可以使用的环境 -->
		<environment id="default">
			<!-- 使用原生JDBC事务 -->
			<transactionManager type="JDBC"></transactionManager>
			<dataSource type="POOLED">
				<property name="driver" value="com.mysql.jdbc.Driver" />
				<property name="url"
					value="jdbc:mysql://192.168.253.132:13306/testDB?useSSL=false" />
				<property name="username" value="test" />
				<property name="password" value="test1" />
			</dataSource>
		</environment>
	</environments>

<!--  指定mapper文件包 -->
	<mappers>
		<package name="com.ntkd.usermapper"/>
	</mappers>
</configuration> 
```

5.java代码 

```java
	public static void main(String[] args) throws IOException {
		
		InputStream is = Resources.getResourceAsStream("mybatis.xml");
		SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(is);
		SqlSession session = factory.openSession();
	
		//通过session.getMapper()获取到mapper接口的实现类（通过代理技术）
		UserMapper um = session.getMapper(UserMapper.class);
		//接口直接调用方法
		List<User> list = um.selectAll();
		
		for(User u : list)
			System.out.println(u);
	}
 
```

**动态SQL**

 

 

 

**缓存**

 只存在于select里，update,delete不存在缓存

**Session缓存**

同一个session里面同一个select id会启用缓存。默认开启。

先查询缓存，缓存没有直接去数据库查找，返回结果并保存到缓存。

缓存的是statement，一个select id 对应一个statement

**SqlSessionFactory缓存**

启用factory二级缓存，同一个factory里面的session都可以使用缓存。

启用方法

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!-- 此处命名空间必须设置为mapper接口的全路径 -->
<mapper namespace="com.ntkd.usermapper.UserMapper">
<!-- 开启factory二级缓存 -->
<cache readOnly="true"></cache>
</mapper>
```

 当session select查询后，session.close()或者commit()后将把缓存放到二级缓存,查询先去二级缓存查 

**ResultMap** 

 **1.单表映射**

```xml
	<resultMap type="com.ntkd.userpojo.User" id="mymap1">
	<!-- column：数据库查出来的字段，property：类里面的字段,可以不一样-->
	<!-- 主键 -->
	<id column="uid" property="uid" />
	<!-- 其他字段 -->
	<result column="uname" property="uname" />
	<result column="pwd" property="pwd" />
	<result column="sex" property="sex" />
	<result column="age" property="age" />
	<result column="birthday" property="birthday" />
	</resultMap>
	
	<select id="selectAllResultMap" resultMap="mymap1" >
		select * from t_user where uid=1
	</select> 
```

 **2.1一个类包含另一个类，一一对应（ 方法）。** 

被包含类完成selectById()查询功能，包含的类mapper如下

```xml
<resultMap type="com.ntkd.userpojo.UserConn" id="mymapapple">
        <!-- column：数据库查出来的字段，property：类里面的字段,可以不一样-->
		<id column="uid" property="uid" />
		<result column="uname" property="uname" />
		<result column="pwd" property="pwd" />
		<result column="sex" property="sex" />
		<result column="age" property="age" />
		<result column="birthday" property="birthday" />
		<result column="appleid" property="appleid" />
		<!-- <association> 装配一个对象时使用
            property: 对象在类中的属性名
            select:通过哪个查询能得到这个对象的信息
            column: 把当前表的哪个列的值做为参数传递给另一个查询
            -->
		<association column="appleid" property="appleConn"
			select="com.ntkd.usermapper.AppleConnMapper.selectById"
			 ></association>
	</resultMap>

	<select id="selectAllResultMapApple" resultMap="mymapapple">
		select * from
		t_user1 where uid=30
	</select>
```

 **2.2一个类包含另一个类，一一对应（自动映射方法）。**

需要查出来的数据的列名称，和包含类里面的属性名.被包含属性名一致

比如这个UserConn有个属性private AppleConn appleConn;

AppleConn有属性id，name

那么列名要为appleConn.id，用tab键上面的引号括起来，防止特殊字符导致sql执行失败

 

```xml
<select id="selectAllResultMapApple1" resultType="com.ntkd.userpojo.UserConn">
		SELECT
			s.uid,s.uname,s.pwd,s.sex,s.age,s.birthday,s.appleid,
			t.id `appleConn.id`,t.NAME `appleConn.name`
		FROM
			t_user1 s LEFT JOIN t_apple t ON s.appleid = t.id
</select>
```

 **3.一个类包含另一个list类(配置查询方法)** 

被包含类完成selectById()查询功能，包含的类mapper如下

```xml
	<resultMap type="com.ntkd.userpojo.AppleConn" id="ddddd">
		<id column="id" property="id" />
		<result column="name" property="name" />
		<!--    
		    <collection/>表示集合
		    property为类的属性名
            column为查询字段
            ofType为List<>中的泛型类型
            select为需要使用哪个查询方法
		-->
		<collection property="list" column="id" ofType="com.ntkd.userpojo.UserConn"
			select="com.ntkd.usermapper.UserConnMapper.selectByAppleid">
		</collection>
	</resultMap>
	
	<select id="selectAppleList" resultMap="ddddd">
		select * from t_apple
	</select>
```

 4**.一个类包含另一个list类(联合查询方法)**

联合查询出来的包含类的重复数据，通过制定包含类的<id>，他会自己分类。

```xml
<resultMap id="eeeee" type="com.ntkd.userpojo.AppleConn" >
		<id column="id" property="id" />
		<result column="name" property="name" />
		<collection property="list" ofType="com.ntkd.userpojo.UserConn">
			<id column="uid" property="uid" />
			<result column="uname" property="uname" />
			<result column="pwd" property="pwd" />
			<result column="sex" property="sex" />
			<result column="age" property="age" />
			<result column="birthday" property="birthday" />
			<result column="appleid" property="appleid" />
		</collection>
	</resultMap>
	<select id="selectAppleList1" resultMap="eeeee">
		select * from t_apple t left join t_user1 s on t.id=s.appleid
	</select> 
```

 

### **注解**

 注解适合单表操作，不适合动态sql

注解可以和mapper.xml共存

 在接口mapper中，使用getMapper得到接口实例化对象后调用方法即可。

```java
	@Select("select * from t_user")
	List<User> selectAllAnno();
	
	@Insert("insert into t_user values(default,#{uname},#{pwd},#{sex},#{age},#{birthday})")
	int insertUserAnno(User u);
	
	@Update("update t_user set uname=#{uname} where uid=#{uid}")
	int updateUserAnno(User u);
	
	@Delete("delete from t_user where uid=#{uid}")
	int deleteUserAnno(User u);
	
	//一个类中包含另一个单个类信息
	@Select(" SELECT " + 
			" s.uid,s.uname,s.pwd,s.sex,s.age,s.birthday,s.appleid, " + 
			" t.id `appleConn.id`,t.NAME `appleConn.name` " + 
			" FROM " + 
			" t_user1 s LEFT JOIN t_apple t ON s.appleid = t.id ")
	List<UserConn> selectAllAnno();   
```

 