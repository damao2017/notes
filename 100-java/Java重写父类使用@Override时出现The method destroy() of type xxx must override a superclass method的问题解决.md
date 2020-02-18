### Java重写父类使用@Override时出现The method destroy() of type xxx must override a superclass method的问题解决

#### 1、把JDK版本改成1.6以上的。

 ﻿项目属性-Java Build Path-Libraries

jre system library 修改成1.8

#### 2、把Compiler改成1.6以上的。

﻿﻿项目属性-Java Compiler

点击右侧的configure workspace settings 把里面设置成1.8，然后出来以后就会加载刚刚配置的1.8

#### 3、把java facets里面的java改成1.6以上的。

 ﻿项目属性-Project Facets

java的版本改成1.8

