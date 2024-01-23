# JVM基础知识

## 什么是JVM

<img src="C:\Users\29579\AppData\Roaming\Typora\typora-user-images\image-20240120161624018.png" alt="image-20240120161624018" style="zoom: 50%;" />

字节码指令无法在计算机上直接运行，使用java虚拟机将字节码转化为机器码，这一过程称为解释

## JVM的功能

![image-20240120161906102](C:\Users\29579\AppData\Roaming\Typora\typora-user-images\image-20240120161906102.png)

java需要经过虚拟机这一步，主要是为了支持跨平台特性

由于JVM需要实时解释虚拟机指令，不做任何优化性能不如直接运行机器码的C、C++等语言。

![image-20240120162122077](C:\Users\29579\AppData\Roaming\Typora\typora-user-images\image-20240120162122077.png)

JVM提供了即时编译（Just-In-Time 简称JIT）进行性能的优化，最终能达到接近C、C++语言的运行性能甚至在特定场景下实现超越

常见的JVM有以下几种

![image-20240120162305181](C:\Users\29579\AppData\Roaming\Typora\typora-user-images\image-20240120162305181.png)

# 字节码文件

字节码文件的主要组成部分：

- 基础信息：魔数、字节码文件对应的Java版本号常量池访问标识(public final等等)父类和接口
- 常量池：保存了字符串常量、类或接口名、字段名主要在字节码指令中使用
- 字段：当前类或接口中声明的字段信息
- 方法：当前类或接口声明的方法信息
- 属性：类的属性，比如源码的文件名、内部类的列表等

## 基本信息

![image-20240120164050146](C:\Users\29579\AppData\Roaming\Typora\typora-user-images\image-20240120164050146.png)

![image-20240120164304730](C:\Users\29579\AppData\Roaming\Typora\typora-user-images\image-20240120164304730.png)

![image-20240120165342111](C:\Users\29579\AppData\Roaming\Typora\typora-user-images\image-20240120165342111.png)

## 常量池

字节码文件中常量池的作用：避免相同的内容重复定义，节省空间。

![image-20240120170208262](C:\Users\29579\AppData\Roaming\Typora\typora-user-images\image-20240120170208262.png)

## 方法

![image-20240120172519560](C:\Users\29579\AppData\Roaming\Typora\typora-user-images\image-20240120172519560.png)

![image-20240120172558066](C:\Users\29579\AppData\Roaming\Typora\typora-user-images\image-20240120172558066.png)

![image-20240120172737465](C:\Users\29579\AppData\Roaming\Typora\typora-user-images\image-20240120172737465.png)

iload将局部变量表中的数据复制一份到操作数栈中

istore将数存到操作数栈中

## 玩转字节码常用工具

![image-20240120192747251](C:\Users\29579\AppData\Roaming\Typora\typora-user-images\image-20240120192747251.png)

一般用于在服务器中查看class信息，可以使用javap -v class文件路径 > 路径/xx.txt （'>'表示将前一个命令输出的内容保存到指定的文件当中）

**使用idea中的jclasslib插件进行查看**

每次修改代码之后，需要点击build -> recompile重新进行编译，并刷新插件

![image-20240120194159956](C:\Users\29579\AppData\Roaming\Typora\typora-user-images\image-20240120194159956.png)

**阿里arthas**
![image-20240120194459542](C:\Users\29579\AppData\Roaming\Typora\typora-user-images\image-20240120194459542.png)

![image-20240120194939985](C:\Users\29579\AppData\Roaming\Typora\typora-user-images\image-20240120194939985.png)

使用java -jar arthas-boot.jar来启动arthas（需要将jar包放在工作目录下）

# 类加载器

## 类的生命周期

描述一个类的加载、使用、卸载的过程

![image-20240120201614252](C:\Users\29579\AppData\Roaming\Typora\typora-user-images\image-20240120201614252.png)

#### 加载阶段

- 加载阶段

  - 加载(Loading)阶段第一步是类加载器根据类的全限定名通过不同的渠道以二进制流的方式获取字节码信息。
  - 类加载器在加载完类之后，Java虚拟机会将字节码中的信息保存到方法区中。
  - 加载器在加载完类之后，Java虚拟机会将字节码中的信息保存到内存的方法区中。（生成一个InstanceKlass对象，保存类的所有信息，里边还包含实现特定功能比如多态的信息。）

  ![image-20240120202641293](C:\Users\29579\AppData\Roaming\Typora\typora-user-images\image-20240120202641293.png)

  - 同时，Java虚拟机还会在堆中生成一份与方法区中数据类似的java.lang.Class对象。（作用是在Java代码中去获取类的信息以及存储静态字段的数据（JDK8及之后）。）

  ![image-20240120202821160](C:\Users\29579\AppData\Roaming\Typora\typora-user-images\image-20240120202821160.png)

jdk1.8之前static字段是存在方法区的，1.8之后static字段放在堆区

为什么要同时设置方法区和堆区？

方法区使用c++实现的不允许程序员直接访问，对于开发者来说，**只需要访问堆中的Class对象而不需要访问方法区中所有信息。这样Java虚拟机就能很好地控制开发者访问数据的范围。**

**查看java进程名称以及进程id的命令**:jps

![image-20240121162254007](C:/Users/29579/AppData/Roaming/Typora/typora-user-images/image-20240121162254007.png)

当一个jar包中有多个启动类时，使用java -cp指定启动类进行启动，而只有一个启动类时，只需要使用java -jar进行启动

#### 连接阶段

包括验证、准备和解析阶段

![image-20240121162555094](C:/Users/29579/AppData/Roaming/Typora/typora-user-images/image-20240121162555094.png)

![image-20240121162654401](C:/Users/29579/AppData/Roaming/Typora/typora-user-images/image-20240121162654401.png)

![image-20240121163323746](C:/Users/29579/AppData/Roaming/Typora/typora-user-images/image-20240121163323746.png)

![image-20240121163357642](C:/Users/29579/AppData/Roaming/Typora/typora-user-images/image-20240121163357642.png)

如果是final修饰的基本数据类型，则没初始为默认值的阶段，直接进行赋值

![image-20240121163458945](C:/Users/29579/AppData/Roaming/Typora/typora-user-images/image-20240121163458945.png)

![image-20240121164106018](C:/Users/29579/AppData/Roaming/Typora/typora-user-images/image-20240121164106018.png)

#### 初始化阶段

![image-20240121165545801](C:/Users/29579/AppData/Roaming/Typora/typora-user-images/image-20240121165545801.png)

![image-20240121170023477](C:/Users/29579/AppData/Roaming/Typora/typora-user-images/image-20240121170023477.png)

![image-20240121170041253](C:/Users/29579/AppData/Roaming/Typora/typora-user-images/image-20240121170041253.png)

![image-20240121170341415](C:/Users/29579/AppData/Roaming/Typora/typora-user-images/image-20240121170341415.png)

<img src="C:/Users/29579/AppData/Roaming/Typora/typora-user-images/image-20240121170649442.png" alt="image-20240121170649442" style="zoom:67%;" />

![image-20240121171618819](C:/Users/29579/AppData/Roaming/Typora/typora-user-images/image-20240121171618819.png)

因此，初始化阶段不一定存在

-----

有继承的初始化阶段

![image-20240121172035498](C:/Users/29579/AppData/Roaming/Typora/typora-user-images/image-20240121172035498.png)

![image-20240121172118098](C:/Users/29579/AppData/Roaming/Typora/typora-user-images/image-20240121172118098.png)

----

**数组的创建不会导致数组中元素的类进行初始化**

![image-20240121172306189](C:/Users/29579/AppData/Roaming/Typora/typora-user-images/image-20240121172306189.png)

![image-20240121172515899](C:/Users/29579/AppData/Roaming/Typora/typora-user-images/image-20240121172515899.png)
