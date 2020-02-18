# nacos 修改登录密码

nacos默认使用apache derby数据库，需要熟悉derby数据库

修改webui登录密码

```java
//下载源码，打开com.alibaba.nacos.console.utils.PasswordEncoderUtil﻿​

package com.alibaba.nacos.console.utils;

import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
/**
 * Password encoder tool
 *
 * @author nacos
 */
public class PasswordEncoderUtil {

    public static void main(String[] args) {
        //设定自己的登录密码
        System.out.println(new BCryptPasswordEncoder().encode("kdzx123"));
    }
}

```

 然后修改nacos下的users表，将生成的密码更新进去 