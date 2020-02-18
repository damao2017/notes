### jackson yaml处理

```xml
<dependencies>
    <dependency>
        <groupId>com.fasterxml.jackson.dataformat</groupId>
        <artifactId>jackson-dataformat-yaml</artifactId>
        <version>2.3.0</version>
    </dependency>
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.2.3</version>
    </dependency>
    <dependency>
        <groupId>org.apache.commons</groupId>
        <artifactId>commons-lang3</artifactId>
        <version>3.4</version>
    </dependency>
</dependencies>
```

```yaml
# Details of a user
---
name: Test User
age: 30
address:
  line1: My Address Line 1
  line2: Address line 2
  city: Washington D.C.
  zip: 20000
roles: 
  - User
  - Editor
```

```java
package com.mms.mja.blog.demo.yaml;

import java.util.Map;
public class User {
    private String name;
    private int age;
    private Map<String, String> address;
    private String[] roles;
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
    public Map<String, String> getAddress() {
        return address;
    }
    public void setAddress(Map<String, String> address) {
        this.address = address;
    }
    public String[] getRoles() {
        return roles;
    }
    public void setRoles(String[] roles) {
        this.roles = roles;
    }
}
```

```java
package com.mms.mja.blog.demo.yaml;

import java.io.File;
import org.apache.commons.lang3.builder.ReflectionToStringBuilder;
import org.apache.commons.lang3.builder.ToStringStyle;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.dataformat.yaml.YAMLFactory;
public class YamlTesting {
    public static void main(String[] args) {
        ObjectMapper mapper = new ObjectMapper(new YAMLFactory());
        try {
            User user = mapper.readValue(new File("user.yaml"), User.class);
            System.out.println(ReflectionToStringBuilder.toString(user,ToStringStyle.MULTI_LINE_STYLE));
        } catch (Exception e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
}
```

