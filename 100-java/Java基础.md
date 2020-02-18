#### Java基础

Java数据类型，继承，重写和重载，四大权限，接口，多态，instenceof，构造方法，this，super，final，static，IO

**数据类型**

原始类型：boolean，char，byte，short，int，long，float，double

包装类型：Boolean，Character，Byte，Short，Integer，Long，Float，Double

int和Integer

int默认是0，Integer默认是null，比如考试考了0分，用int没法区别，用Integer可以区别

Integer还有一些方法可以使用

**继承**

1.只能单继承，一个类继承一个类，不能继承多个类。

2.子类能调用自己的成员变量和方法，也可以调用父类的成员变量和方法。

子类先找自己的成员变量和方法，找不到就找父类的。子类和父类可以有相同的成员变量和方法。

弊端：耦合度太高，类和类之间有关系

**重写和重载**

重写：两个类，子类和父类相同的方法

重载：一个类，方法的参数不一样

**四大权限**

public>protect>default>private

defaule默认不用写

**抽象类（继承体系思想）**

类和方法用abstract修饰，方法没有方法体。不可实例化，只可被子类继承，方法只能被子类重写。

**接口**

使用public abstract关键字修饰方法，可以省略

使用public static final关键字修饰成员变量，必须赋值

类实现接口implements

实现类如果没有全部实现接口方法，那实现类必须定义为抽象类。

可以多实现。一个类实现多个接口。多个接口的方法名必须唯一。

**多态性**

继承和方法的重写

父类（可以是抽象类）或者接口类型 变量名 = new 子类的对象();

成员变量（编译运行都看父类）

编译时候，看父类有没有，

运行时候，使用父类的成员变量

成员方法（编译看父类，运行看子类）

编译时候，看父类有没有

运行时候，运行子类的方法，没有再运行父类的方法 （被static修饰的看父类方法）

**instenceof**

只对有继承关系的可以使用

引用对象比较，判断变量到底是谁的对象

**构造方法**

构造方法：创建对象自动执行，执行一次。

普通方法：创建对象后手工调用执行，可多次调用。

构造方法的第一行，肯定是super()，隐式，默认不显示

**this**

指向本类对象，区分本类中同名的类成员变量和局部变量，还可以在构造器中调用其他构造函数

**super**

指向父类对象

**final**

final修饰类，类不能被继承

final修饰方法，方法不能被重写

final修饰变量，变量不能被重新赋值

final修饰成员变量，只能通过等于或者构造方法赋值

**static**

共享数据

修饰成员变量，该变量属于这个类的共享数据，不属于某个实例化类。

修饰方法，方法中不能使用非静态变量，因为生命周期，静态的生命周期优先于非静态的生命周期。不能写this,super

看到day13

 

 

 

**IO**

字节流可以处理所有文件

字节输入流：InputStream

FIle->FileInputStream->BufferedInputStream

字节输出流：OutputString

FIle->FileOutputStream->BufferedOutputStream

 

字符流只能处理文本文件

字符输入流：Reader

FIle->FileReader->BufferedReader

字符输出流：Writer

FIle->FileWriter->BufferedWriter

 

字节流InputStream-----InputStreamReader(解码)---->字符流Reader

字节流OutputStream-----OutputStreamReader(编码)---->字符流Writer

 

 