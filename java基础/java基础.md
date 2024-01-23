# java学习路线

- [ ] java基础：基础语法、异常、IO流、集合类、多线程、泛型
- [ ] 学习java8：stream API、lambda表达式、新日期时间API、接口默认方法（看宋洪康java部分）
- [ ] 看java核心技术卷1、刷牛客java练习题
- [ ] MySQL
- [ ] Spring、SpringMVC、SpringBoot、MyBatis、MyBatis Plus、Spring Security、Maven
- [ ] 开发规范，看大厂规范手册
- [ ] Linux：系统安装、环境变量、文件管理、用户管理、内存管理、磁盘管理、进程管理、  网络管理、软件包管理、服务管理、日志管理、Linux内核、shell脚本编程、VIM使用
- [ ] 设计模式: 创建型模式、结构型模式、行为型模式（书籍：图解设计模式）
- [ ] 中间件：Redis缓存、RabbitMQ消息队列、Nginx网关
- [ ] Netty网络编程（不需要太多时间，看看思想）
- [ ] 微服务：Dubbo、微服务、接口管理
- [ ] 容器，会用Docker/K8S部署项目和服务即可
- [ ] 项目实战
- [ ] 并发编程
- [ ] JVM（书籍：深入理解Java虚拟机）、架构设计：分布式、高并发、高可用

# 八股

## 重载和重写的区别？重载的方法能否根据返回类型进行区分？

方法的重载和重写都是实现多态的方式，区别在于前者实现的是编译时的多态性，而后者实现的是运行时的多态性。

重载：发生在同一个类中，方法名相同参数列表不同（参数类型不同、个数不同、顺序不同），与方法返回值和访问修饰符无关，即重载的方法不能根据返回类型进行区分

重写：发生在父子类中，方法名、参数列表必须相同，**返回值小于等于父类**，抛出的异常小于等于父类，**访问修饰符大于等于父类**（里氏代换原则）；如果父类方法访问修饰符为private则子类中就不是重写。

## ==和equals的区别是什么？

三=：它的作用是判断两个对象的地址是不是相等。即，判断两个对象是不是同一个对象。（基本数据类型三=比较的是值，引用数据类型==比较的是内存地址）

equals（）：它的作用也是判断两个对象是否相等。但它一般有两种使用情况：情况1：类没有覆盖equals（）方法。则通过equals（）比较该类的两个对象时，等价于通过"=="比较这两个对象。

情况2：类覆盖了equals（）方法。一般，我们都覆盖equals（）方法来两个对象的内容相等；若它们的内容相等，则返回true（即，认为这两个对象相等）。

# Java基本知识

java语言本身不能对操作系统底层进行访问和操作，但是可以通过JNI接口调用其他语言来实现对底层的访问，例如以下方法

```java
public static native int identityHashCode(Object x);//以内存计算
//hashCode的方式，natice关键字说明其修饰的方法是一个原生态方法，方法对应的实现
//不是在当前文件，而是在用其他语言（如c和c++）实现的文件中
```

## 基础知识

### JDK

Java Development Kit（Java开发工具包）

JDK = JRE + java的开发工具(Java、javac、javadoc、javap等)

### JRE

运行java字节码的虚拟机(java运行环境)

JRE = JVM + Java的核心类库

> java中一个文件最多只能有一个public类，其他类个数不限
> 
> 如果源文件包含一个public类，则文件名必须按该类名命名

### 杂项

在jdk目录下，可执行文件javac是编译器，可执行文件java是虚拟机

如果想要在任意文件夹下都可以运行java，需要将JAVA_HOME的bin目录添加到path中

### 数据类型

数据类型包括基本数据类型和引用数据类型

#### 基本数据类型

基本数据类型包括数值型和字符型

##### 数值型

数值型包括整数类型和浮点类型

**整数类型**：byte, short, int, long, float, double, char, boolean

> 1. java各整数类型有固定的方位和字段长度，不受具体OS的影响，以保证java程序的可移植性
> 
> 2. java的整型常量默认为int型，声明long型常量须后加'l'或'L'

**浮点类型**: float, double

> 1. 单精度float占4个字节，双精度double占8个字节
> 
> 2. 浮点型常量默认为double型，声明float型常量，须后加'f'或'F'

##### 字符型

字符类型可以表示单个字符或汉字，占2个字节

> 1. Java中char本质是一个整数，在输出时，是unicode码对应的字符
> 
> 2. 可以直接给char赋一个整数
> 
> 3. char类型可以进行运算，相当于一个整数

基本类型和引用类型的变量

```java
        char c = 'a';
        byte b = 30;
        boolean bo = true;
        short sh = 30;
        int in = 30;
        long lo = 30L;
        float fl = 0.45f;
        final int fi = 40;
        // fi = 3;
        int[] a = {1, 2, 3, 5};

        System.out.println(Byte.SIZE);
        System.out.println(Character.SIZE);

        System.out.printf("%.1f", fl);
        for(int i: a) {
            System.out.println(i);
        }
        System.out.println(Arrays.toString(a));
```

## 数组

### 实现自定义排序

使用匿名内部类或者自定义Comparator类

```java
class MyComparator implements Comparator<Integer> {

    @Override
    public int compare(Integer o1, Integer o2) {
        return o2 - o1;
    }
}

public class demo2 {

    public static void main(String[] args) {
        Integer[] a = {1, 4, 23, 2, 1};
        Integer[] b = new Integer[a.length];

        Arrays.sort(a);
        System.out.println(Arrays.toString(a));
        System.out.println(Arrays.toString(b) + "---" + (a==b));
        //用匿名内部类实现自定义排序
        Arrays.sort(a, new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o2 - o1;
            }
        });
        System.out.println(Arrays.toString(a));
    }
}
```

### 数组拷贝

```java
System.arraycopy(a, 0, b, 0, a.length);
```

```java
        Integer[] a = {1, 4, 23, 2, 1};
        Integer[] b = new Integer[a.length];
        System.arraycopy(a, 0, b, 0, a.length);
        Arrays.sort(a);
        System.out.println(Arrays.toString(a));
        System.out.println(Arrays.toString(b) + "---" + (a==b));

        Integer[] c = new Integer[6];
        System.out.println(Arrays.toString(c));
        c = Arrays.copyOf(a, a.length);
        System.out.println(Arrays.toString(c));
```

### 数组打印

```java
        //二维数组打印
        int[][] ts = {
                {1, 13, 2, 24},
                {11, 23, 32, 44}
        };

        System.out.println(ts);
        //一维数组打印方式
        System.out.println(Arrays.toString(ts));
        //多维数组打印方式
        System.out.println(Arrays.deepToString(ts));
```

运行结果

```java
[[I@b4c966a
[[I@2f4d3709, [I@4e50df2e]
[[1, 13, 2, 24], [11, 23, 32, 44]]
```

## 异常

**idea中使用ctrl + alt + T可以快速生成代码**

Java的异常类型包括错误和异常两大类型

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2023-12-30-21-17-58-image.png" title="" alt="" width="688">

### 错误（Error）

Java虚拟机无法解决的严重问题，如：JVM系统内部错误，资源耗尽等严重情况。比如：stackoverflow和out of memory

### 异常

分为运行时异常（程序运行时发生的异常）、编译时异常（编程时，编译器检查出的异常)

**运行时异常**

> 1. 运行时异常，编译器检查不出来。一般是值编程时的逻辑错误，是程序员应该避免其出现的异常。java.lang.RuntimeException类及它的子类都是运行时异常
> 
> 2. 对于运行时异常，可以不作处理，因为种类异常很普遍，若全处理可能会对程序的可读性和运行效率产生影响
> 
> 3. 编译时异常，是编译器要求必须处置的异常

**编译时异常**

> 编译异常是指在编译期间，必须处理的异常，否则代码不能通过编译

![](C:\Users\29579\AppData\Roaming\marktext\images\2023-12-30-21-30-49-image.png)

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2023-12-30-21-31-23-image.png" title="" alt="" width="612">

#### 异常处理

异常处理就是异常发生时，对异常的处理方式

> 1. try-catch-finally
>    
>    程序员在代码中捕获发生的异常，自行处理
> 
> 2. throws
>    
>    将发生的异常抛出，交给调用者（方法）来处理，最顶级的处理者就是JVM

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2023-12-30-21-35-35-image.png" title="" alt="" width="600">

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2023-12-30-21-35-10-image.png" title="" alt="" width="630">

#### 自定义异常

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2023-12-30-21-38-27-image.png" title="" alt="" width="578">

自定义异常步骤

![](C:\Users\29579\AppData\Roaming\marktext\images\2023-12-30-21-39-14-image.png)

```java
//1. 一般情况下，我们自定义异常是继承 RuntimeException
//2. 即把自定义异常做成 运行时异常，好处时，我们可以使用默认的处理机制
//3. 即比较方便public class AgeException extends RuntimeException{
    public AgeException(String message) {
        super(message);
    }
}
```

## 类

在idea中使用alt+insert实现快速代码生成

### 包装类

针对八种基本数据类型相应的引用类型—包装类，有了类的特点，就可以调用类中的方法

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2023-12-30-21-46-18-image.png" title="" alt="" width="369">

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2023-12-30-21-47-09-image.png" title="" alt="" width="530">

```java
//手动装箱、拆箱
//jdk5 前是手动装箱和拆箱
//手动装箱 int->Integer
int n1 = 100;
Integer integer = new Integer(n1);
Integer integer1 = Integer.valueOf(n1);
//手动拆箱
//Integer -> int
int i = integer.int

//jdk5 后，就可以自动装箱和自动拆箱
int n2 = 200;
//自动装箱 int->Integer
Integer integer2 = n2; //底层使用的是 Integer.valueOf(n2)
//自动拆箱 Integer->int
int n3 = integer2; //底层仍然使用的是 intValue()方法Value()
```

```java
System.out.println(Integer.MIN_VALUE); //返回最小值
System.out.println(Integer.MAX_VALUE);//返回最大值
System.out.println(Character.isDigit('a'));//判断是不是数字
System.out.println(Character.isLetter('a'));//判断是不是字母
System.out.println(Character.isUpperCase('a'));//判断是不是大写
System.out.println(Character.isLowerCase('a'));//判断是不是小写
System.out.println(Character.isWhitespace('a'));//判断是不是空格
System.out.println(Character.toUpperCase('a'));//转成大写
System.out.println(Character.toLowerCase('A'));//转成小写
```

```java
public static void main(String[] args) {
Integer i = new Integer(1);
Integer j = new Integer(1);
System.out.println(i == j); //False
//所以，这里主要是看范围 -128 ~ 127 就是直接返回
/*
老韩解读
//1. 如果 i 在 IntegerCache.low(-128)~IntegerCache.high(127),就直接从数组返回
//2. 如果不在 -128~127,就直接 new Integer(i)
public static Integer valueOf(int i) {
if (i >= IntegerCache.low && i <= IntegerCache.high)
return IntegerCache.cache[i + (-IntegerCache.low)];
return new Integer(i);
}
*/
Integer m = 1; //底层 Integer.valueOf(1); -> 阅读源码
Integer n = 1;//底层 Integer.valueOf(1);
System.out.println(m == n); //T
//所以，这里主要是看范围 -128 ~ 127 就是直接返回
//，否则，就 new Integer(xx);
Integer x = 128;//底层 Integer.valueOf(1);
Integer y = 128;//底层 Integer.valueOf(1);
System.out.println(x == y);//False
}
}
```

### String类

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2023-12-30-21-58-48-image.png" title="" alt="" width="521">

**创建string对象的两种方式**

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2023-12-30-22-07-21-image.png" title="" alt="" width="431">

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2023-12-30-22-07-38-image.png" title="" alt="" width="481">

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2023-12-30-22-09-24-image.png" title="" alt="" width="459">

String.intern()的使用，其作用是将指定的字符串对象的引用保存在字符串常量池中：

- 如果字符串常量池中保存了对应的字符串对象的引用，则直接返回引用

- 如果字符串常量池中没有保存对应的字符串对象的引用，那就在常量池中创建一个指向该字符串对象的引用并返回

```java
String s = "this";
s.hashCode()和System.identityHashCode()返回的内容是不一样的，前者是根据内容
//计算的hash而后者是根据对象的地址计算的
```

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2023-12-30-22-11-06-image.png" title="" alt="" width="532">

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2023-12-30-22-12-03-image.png" title="" alt="" width="509">

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2023-12-30-22-14-05-image.png" title="" alt="" width="514">

#### StringBuilder和StringBuffer

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2023-12-30-22-15-12-image.png" title="" alt="" width="553">

string类的常见方法

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2023-12-30-22-15-46-image.png" title="" alt="" width="540">

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2023-12-30-22-17-05-image.png" title="" alt="" width="502">

```java
//1. %s , %d , %.2f %c 称为占位符
//2. 这些占位符由后面变量来替换
//3. %s 表示后面由 字符串来替换
//4. %d 是整数来替换
//5. %.2f 表示使用小数来替换，替换后，只会保留小数点两位, 并且进行四舍五入的处理
//6. %c 使用 char 类型来替换
String formatStr = "我的姓名是%s 年龄是%d，成绩是%.2f 性别是%c.希望大家喜欢我！";
String info2 = String.format(formatStr, name, age, score, gender);
```

stringbuilder和stringbuffer

<img title="" src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2023-12-30-22-19-54-image.png" alt="" width="519">

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2023-12-30-22-21-15-image.png" title="" alt="" width="554">

```java
public class StringBufferMethod {
public static void main(String[] args) {
StringBuffer s = new StringBuffer("hello");
//增
s.append(',');// "hello,"
s.append("张三丰");//"hello,张三丰"
s.append("赵敏").append(100).append(true).append(10.5);//"hello,张三丰赵敏 100true10.5" System.out.println(s);//"hello,张三丰赵敏 100true10.5"
//删
/*
* 删除索引为>=start && <end 处的字符
* 解读: 删除 11~14 的字符 [11, 14)
*/
s.delete(11, 14);
System.out.println(s);//"hello,张三丰赵敏 true10.5"
//改
//老韩解读，使用 周芷若 替换 索引 9-11 的字符 [9,11)
s.replace(9, 11, "周芷若");
System.out.println(s);//"hello,张三丰周芷若 true10.5"
//查找指定的子串在字符串第一次出现的索引，如果找不到返回-1
int indexOf = s.indexOf("张三丰");
System.out.println(indexOf);//6
//插
韩顺平循序渐进学 Java 零基础
第 560页
//老韩解读，在索引为 9 的位置插入 "赵敏",原来索引为 9 的内容自动后移
s.insert(9, "赵敏");
System.out.println(s);//"hello,张三丰赵敏周芷若 true10.5"
//长度
System.out.println(s.length());//22
System.out.println(s);
}
}
```

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2023-12-30-22-37-03-image.png" title="" alt="" width="510">

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2023-12-30-22-38-35-image.png" title="" alt="" width="535">

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2023-12-30-22-40-09-image.png" title="" alt="" width="533">

### Math类

```java
public class MathMethod {
public static void main(String[] args) {
//看看 Math 常用的方法(静态方法)
//1.abs 绝对值
int abs = Math.abs(-9);
System.out.println(abs);//9
韩顺平循序渐进学 Java 零基础
第 569页
//2.pow 求幂
double pow = Math.pow(2, 4);//2 的 4 次方
System.out.println(pow);//16
//3.ceil 向上取整,返回>=该参数的最小整数(转成 double);
double ceil = Math.ceil(3.9);
System.out.println(ceil);//4.0
//4.floor 向下取整，返回<=该参数的最大整数(转成 double)
double floor = Math.floor(4.001);
System.out.println(floor);//4.0
//5.round 四舍五入 Math.floor(该参数+0.5)
long round = Math.round(5.51);
System.out.println(round);//6
//6.sqrt 求开方
double sqrt = Math.sqrt(9.0);
System.out.println(sqrt);//3.0
//7.random 求随机数
// random 返回的是 0 <= x < 1 之间的一个随机小数
// 思考：请写出获取 a-b 之间的一个随机整数,a,b 均为整数 ，比如 a = 2, b=7
// 即返回一个数 x 2 <= x <= 7
// 老韩解读 Math.random() * (b-a) 返回的就是 0 <= 数 <= b-a
// (1) (int)(a) <= x <= (int)(a + Math.random() * (b-a +1) )
// (2) 使用具体的数给小伙伴介绍 a = 2 b = 7
// (int)(a + Math.random() * (b-a +1) ) = (int)( 2 + Math.random()*6)
// Math.random()*6 返回的是 0 <= x < 6 小数
// 2 + Math.random()*6 返回的就是 2<= x < 8 小数
韩顺平循序渐进学 Java 零基础
第 570页
// (int)(2 + Math.random()*6) = 2 <= x <= 7
// (3) 公式就是 (int)(a + Math.random() * (b-a +1) )
for(int i = 0; i < 100; i++) {
System.out.println((int)(2 + Math.random() * (7 - 2 + 1)));
}
//max , min 返回最大值和最小值
int min = Math.min(1, 9);
int max = Math.max(45, 90);
System.out.println("min=" + min);
System.out.println("max=" + max);
}
}
```

### Arrays类

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2023-12-30-22-43-09-image.png" title="" alt="" width="505">

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2023-12-30-22-43-43-image.png" title="" alt="" width="391">

```java
Arrays.sort(arr, new Comparator() {
@Override
public int compare(Object o1, Object o2) {
Integer i1 = (Integer) o1;
Integer i2 = (Integer) o2;
return i2 - i1;
}
});
//copyOf 数组元素的复制
// 老韩解读
//1. 从 arr 数组中，拷贝 arr.length 个元素到 newArr 数组中
//2. 如果拷贝的长度 > arr.length 就在新数组的后面 增加 null
//3. 如果拷贝长度 < 0 就抛出异常 NegativeArraySizeException
//4. 该方法的底层使用的是 System.arraycopy
```

### System类

System类的常见方法

<img title="" src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-01-19-52-52-image.png" alt="" width="426">

### 日期类

#### 第一代日期类

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-01-20-03-47-image.png" title="" alt="" width="381">

#### 第二代日期类

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-01-20-15-14-image.png" title="" alt="" width="491">

```java
public class Calendar_ {
public static void main(String[] args) {
//老韩解读
//1. Calendar 是一个抽象类， 并且构造器是private
//2. 可以通过 getInstance() 来获取实例
//3. 提供大量的方法和字段提供给程序员
韩顺平循序渐进学Java 零基础
第593页
//4. Calendar 没有提供对应的格式化的类，因此需要程序员自己组合来输出(灵活)
//5. 如果我们需要按照 24 小时进制来获取时间， Calendar.HOUR ==改成=> Calendar.HOUR_OF_DAY
Calendar c = Calendar.getInstance(); //创建日历类对象//比较简单，自由
System.out.println("c=" + c);
//2.获取日历对象的某个日历字段
System.out.println("年：" + c.get(Calendar.YEAR));
// 这里为什么要 + 1, 因为Calendar 返回月时候，是按照 0 开始编号
System.out.println("月：" + (c.get(Calendar.MONTH) + 1));
System.out.println("日：" + c.get(Calendar.DAY_OF_MONTH));
System.out.println("小时：" + c.get(Calendar.HOUR));
System.out.println("分钟：" + c.get(Calendar.MINUTE));
System.out.println("秒：" + c.get(Calendar.SECOND));
//Calender 没有专门的格式化方法，所以需要程序员自己来组合显示
System.out.println(c.get(Calendar.YEAR) + "-" + (c.get(Calendar.MONTH) + 1) + "-" +
c.get(Calendar.DAY_OF_MONTH) +
" " + c.get(Calendar.HOUR_OF_DAY) + ":" + c.get(Calendar.MINUTE) + ":" + c.get(Calendar.SECOND) );
}
}
```

#### 第三代日期类

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-01-20-20-04-image.png" title="" alt="" width="498">

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-01-20-20-51-image.png" title="" alt="" width="534">

```java
public class LocalDate_ {
public static void main(String[] args) {
//第三代日期
//老韩解读
//1. 使用now() 返回表示当前日期时间的 对象
LocalDateTime ldt = LocalDateTime.now(); //LocalDate.now();//LocalTime.now()
System.out.println(ldt);
//2. 使用DateTimeFormatter 对象来进行格式化
// 创建 DateTimeFormatter 对象
DateTimeFormatter dateTimeFormatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
String format = dateTimeFormatter.format(ldt);
System.out.println("格式化的日期=" + format);
System.out.println("年=" + ldt.getYear());
System.out.println("月=" + ldt.getMonth());
System.out.println("月=" + ldt.getMonthValue());
System.out.println("日=" + ldt.getDayOfMonth());
System.out.println("时=" + ldt.getHour());
System.out.println("分=" + ldt.getMinute());
System.out.println("秒=" + ldt.getSecond());
LocalDate now = LocalDate.now(); //可以获取年月日
韩顺平循序渐进学Java 零基础
第596页
LocalTime now2 = LocalTime.now();//获取到时分秒
//提供 plus 和 minus 方法可以对当前时间进行加或者减
//看看890 天后，是什么时候 把 年月日-时分秒
LocalDateTime localDateTime = ldt.plusDays(890);
System.out.println("890 天后=" + dateTimeFormatter.format(localDateTime));
//看看在 3456 分钟前是什么时候，把 年月日-时分秒输出
LocalDateTime localDateTime2 = ldt.minusMinutes(3456);
System.out.println("3456 分钟前 日期=" + dateTimeFormatter.format(localDateTime2));
}
```

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-01-20-28-00-image.png" title="" alt="" width="252">

### 内部类

内部类包括：局部内部类、匿名内部类、成员内部类、静态内部类

#### 静态内部类

定义在类内部的静态类，就是静态内部类

```java
public class Outer {
   private static int radius = 1;

   static class StaticInner {
      public void visit() {
         System.out.println("visit outer static variable:" + radius);
      }
   }
}
```

静态内部类可以访问外部类所有的静态变量，而不可访问外部类的非静态变量； 静态内部类的创建方式，new 外部类.静态内部类()，如下：

```java
Outer.StaticInner inner = new Outer.StaticInner();
inner.visit
```

#### 成员内部类

定义在类内部，成员位置上的非静态类，就是成员内部类。

```java
public class Outer {
   private static int radius = 1;
   private int count =2;
   class Inner {
      public void visit() {
      System.out.println("visit outer static variable:" + radius);
      System.out.println("visit outer variable:" + count);
      }
   }
}
```

成员内部类可以访问外部类所有的变量和方法，包括静态和非静态，私有和公 有。成员内部类依赖于外部类的实例，它的创建方式外部类实例.new 内部类()，如 下：

```java
Outer outer = new Outer();
Outer.Inner inner = outer.new Inner();
inner.visit();
```

#### 局部内部类

定义在方法中的内部类，就是局部内部类

```java
public class Outer {
   private int out_a = 1;
   private static int STATIC_b = 2;
   public void testFunctionClass(){
      int inner_c =3;
      class Inner {
         private void fun(){
            System.out.println(out_a);
            System.out.println(STATIC_b);
            System.out.println(inner_c);
         }
      }
     Inner inner = new Inner();
     inner.fun();
     }
   public static void testStaticFunctionClass(){
      int d =3;
      class Inner {
         private void fun(){
 // System.out.println(out_a); 编译错误，定义在静态方法中的局部类不可以访问外部类的实例变量
         System.out.println(STATIC_b);
         System.out.println(d);
      }
   }
   Inner inner = new Inner();
   inner.fun();
   }
}
```

定义在实例方法中的局部类可以访问外部类的所有变量和方法，定义在静态方法 中的局部类只能访问外部类的静态变量和方法。局部内部类的创建方式，在对应 方法内，new 内部类()

#### 匿名内部类

匿名内部类就是没有名字的内部类，日常开发中使用的比较多。

```java
public class Outer {
   private void test(final int i) {
     new Service() {
         public void method() {
           for (int j = 0; j < i; j++) {
             System.out.println("匿名内部类" );
           }
         }
     }.method();
   }
}
//匿名内部类必须继承或实现一个已有的接口
interface Service{
  void method();
}
```

除了没有名字，匿名内部类还有以下特点：

- 匿名内部类必须继承一个抽象类或者实现一个接口。
- 匿名内部类不能定义任何静态成员和静态方法。
- 当所在的方法的形参需要被匿名内部类使用时，必须声明为 final。
- 匿名内部类不能是抽象的，它必须要实现继承的类或者实现的接口的所有抽象方 法。

**局部内部类和匿名内部类访问局部变量的时候，为什么变量必须要加上final呢？ 它内部原理是什么呢？**

```java
public class Outer {
  void outMethod(){
    final int a =10;
    class Inner {
      void innerMethod(){
        System.out.println(a);
      }
    }
  }
}
```

以上例子，为什么要加final呢？是因为生命周期不一致， 局部变量直接存储在 栈中，当方法执行结束后，非final的局部变量就被销毁。而局部内部类对局部变 量的引用依然存在，如果局部内部类要调用局部变量时，就会出错。加了final， 可以确保局部内部类使用的变量与外层的局部变量区分开，解决了这个问题。

## 集合

java集合主要分为两大类，List类和Set类

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-01-20-42-18-image.png" title="" alt="" width="516">

<img title="" src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-01-20-42-43-image.png" alt="" width="373">

### Collection接口

<img title="" src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-01-20-45-04-image.png" alt="" width="457">

```java
public class CollectionMethod {
@SuppressWarnings({"all"})
public static void main(String[] args) {
List list = new ArrayList();
// add:添加单个元素
list.add("jack");
list.add(10);//list.add(new Integer(10))
list.add(true);
System.out.println("list=" + list);
// remove:删除指定元素
//list.remove(0);//删除第一个元素
list.remove(true);//指定删除某个元素
System.out.println("list=" + list);
// contains:查找元素是否存在
System.out.println(list.contains("jack"));//T
// size:获取元素个数
System.out.println(list.size());//2
韩顺平循序渐进学Java 零基础
第604页
// isEmpty:判断是否为空
System.out.println(list.isEmpty());//F
// clear:清空
list.clear();
System.out.println("list=" + list);
// addAll:添加多个元素
ArrayList list2 = new ArrayList();
list2.add("红楼梦");
list2.add("三国演义");
list.addAll(list2);
System.out.println("list=" + list);
// containsAll:查找多个元素是否都存在
System.out.println(list.containsAll(list2));//T
// removeAll：删除多个元素
list.add("聊斋");
list.removeAll(list2);
System.out.println("list=" + list);//[聊斋]
// 说明：以ArrayList 实现类来演示.
}
}
```

Collection 接口遍历元素方式1-使用Iterator(迭代器)

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-01-20-48-44-image.png" title="" alt="" width="501">

<img title="" src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-01-20-50-27-image.png" alt="" width="346">

Collection 接口遍历对象方式2-for 循环增强

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-01-20-54-12-image.png" title="" alt="" width="504">

### List接口

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-01-20-59-27-image.png" title="" alt="" width="521">

```java
public class ListMethod {
@SuppressWarnings({"all"})
public static void main(String[] args) {
List list = new ArrayList();
list.add("张三丰");
list.add("贾宝玉");
韩顺平循序渐进学Java 零基础
第616页
// void add(int index, Object ele):在index 位置插入ele 元素
//在index = 1 的位置插入一个对象
list.add(1, "韩顺平");
System.out.println("list=" + list);
// boolean addAll(int index, Collection eles):从index 位置开始将eles 中的所有元素添加进来
List list2 = new ArrayList();
list2.add("jack");
list2.add("tom");
list.addAll(1, list2);
System.out.println("list=" + list);
// Object get(int index):获取指定index 位置的元素
//说过
// int indexOf(Object obj):返回obj 在集合中首次出现的位置
System.out.println(list.indexOf("tom"));//2
// int lastIndexOf(Object obj):返回obj 在当前集合中末次出现的位置
list.add("韩顺平");
System.out.println("list=" + list);
System.out.println(list.lastIndexOf("韩顺平"));
// Object remove(int index):移除指定index 位置的元素，并返回此元素
list.remove(0);
System.out.println("list=" + list);
// Object set(int index, Object ele):设置指定index 位置的元素为ele , 相当于是替换.
list.set(1, "玛丽");
System.out.println("list=" + list);
// List subList(int fromIndex, int toIndex):返回从fromIndex 到toIndex 位置的子集合
// 注意返回的子集合 fromIndex <= subList < toIndex
韩顺平循序渐进学Java 零基础
第617页
List returnlist = list.subList(0, 2);
System.out.println("returnlist=" + returnlist);
}
```

遍历方式

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-01-21-03-22-image.png" title="" alt="" width="287">

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-01-21-13-24-image.png" title="" alt="" width="578">

<img title="" src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-01-21-14-55-image.png" alt="" width="559">

#### vector

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-01-21-22-53-image.png" title="" alt="" width="506">

#### LinkedList

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-01-21-25-04-image.png" title="" alt="" width="488">

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-01-21-26-04-image.png" title="" alt="" width="607">

LinkedList和ArrayList比较

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-01-21-30-37-image.png" title="" alt="" width="604">

### Set接口

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-01-21-31-21-image.png" title="" alt="" width="556">

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-01-21-32-45-image.png" title="" alt="" width="579">

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-01-21-44-46-image.png" title="" alt="" width="570">

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-01-21-45-21-image.png" title="" alt="" width="301">

```java
public class HashSet01 {
public static void main(String[] args) {
HashSet set = new HashSet();
韩顺平循序渐进学Java 零基础
第645页
//说明
//1. 在执行add 方法后，会返回一个boolean 值
//2. 如果添加成功，返回 true, 否则返回false
//3. 可以通过 remove 指定删除哪个对象
System.out.println(set.add("john"));//T
System.out.println(set.add("lucy"));//T
System.out.println(set.add("john"));//F
System.out.println(set.add("jack"));//T
System.out.println(set.add("Rose"));//T
set.remove("john");
System.out.println("set=" + set);//3 个
//
set = new HashSet();
System.out.println("set=" + set);//0
//4 Hashset 不能添加相同的元素/数据?
set.add("lucy");//添加成功
set.add("lucy");//加入不了
set.add(new Dog("tom"));//OK
set.add(new Dog("tom"));//Ok
System.out.println("set=" + set);
//在加深一下. 非常经典的面试题.
韩顺平循序渐进学Java 零基础
第646页
//看源码，做分析， 先给小伙伴留一个坑，以后讲完源码，你就了然
//去看他的源码，即 add 到底发生了什么?=> 底层机制.
set.add(new String("hsp"));//ok
set.add(new String("hsp"));//加入不了.
System.out.println("set=" + set);
}
}
class Dog { //定义了Dog 类
private String name;
public Dog(String name) {
this.name = name;
}
@Override
public String toString() {
return "Dog{" +
"name='" + name + '\'' +
'}';
}
}
```

#### LinkedHashSet

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-01-22-24-55-image.png" title="" alt="" width="545">

<img title="" src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-01-22-26-02-image.png" alt="" width="550">

### Map接口

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-01-22-30-37-image.png" title="" alt="" width="602">

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-01-23-30-04-image.png" title="" alt="" width="297">

```java
public class MapFor {
public static void main(String[] args) {
Map map = new HashMap();
map.put("邓超", "孙俪");
map.put("王宝强", "马蓉");
map.put("宋喆", "马蓉");
map.put("刘令博", null);
map.put(null, "刘亦菲");
map.put("鹿晗", "关晓彤");
//第一组: 先取出 所有的Key , 通过Key 取出对应的Value
Set keyset = map.keySet();
韩顺平循序渐进学Java 零基础
第670页
//(1) 增强for
System.out.println("-----第一种方式-------");
for (Object key : keyset) {
System.out.println(key + "-" + map.get(key));
}
//(2) 迭代器
System.out.println("----第二种方式--------");
Iterator iterator = keyset.iterator();
while (iterator.hasNext()) {
Object key = iterator.next();
System.out.println(key + "-" + map.get(key));
}
//第二组: 把所有的values 取出
Collection values = map.values();
//这里可以使用所有的Collections 使用的遍历方法
//(1) 增强for
System.out.println("---取出所有的value 增强for----");
for (Object value : values) {
System.out.println(value);
}
//(2) 迭代器
System.out.println("---取出所有的value 迭代器----");
Iterator iterator2 = values.iterator();
while (iterator2.hasNext()) {
Object value = iterator2.next();
韩顺平循序渐进学Java 零基础
第671页
System.out.println(value);
}
//第三组: 通过EntrySet 来获取 k-v
Set entrySet = map.entrySet();// EntrySet<Map.Entry<K,V>>
//(1) 增强for
System.out.println("----使用EntrySet 的 for 增强(第3 种)----");
for (Object entry : entrySet) {
//将entry 转成 Map.Entry
Map.Entry m = (Map.Entry) entry;
System.out.println(m.getKey() + "-" + m.getValue());
}
//(2) 迭代器
System.out.println("----使用EntrySet 的 迭代器(第4 种)----");
Iterator iterator3 = entrySet.iterator();
while (iterator3.hasNext()) {
Object entry = iterator3.next();
//System.out.println(next.getClass());//HashMap$Node -实现-> Map.Entry (getKey,getValue)
//向下转型 Map.Entry
Map.Entry m = (Map.Entry) entry;
System.out.println(m.getKey() + "-" + m.getValue());
}
}
```

## IO流

<img src="C:\Users\29579\AppData\Roaming\Typora\typora-user-images\image-20240118093959541.png" alt="image-20240118093959541" style="zoom: 67%;" />

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-11-10-41-54-image.png" title="" alt="" width="569">

### 常用的文件操作

#### 创建文件对象相关构造器和方法

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-11-10-43-10-image.png" title="" alt="" width="545">

#### 获取文件相关信息

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-11-10-48-14-image.png" title="" alt="" width="581">

```java
public class FileInformation {
public static void main(String[] args) {
}
//获取文件的信息
@Test
public void info() {
//先创建文件对象
File file = new File("e:\\news1.txt");
//调用相应的方法，得到对应信息
System.out.println("文件名字=" + file.getName());
//getName、getAbsolutePath、getParent、length、exists、isFile、isDirectory
System.out.println("文件绝对路径=" + file.getAbsolutePath());
System.out.println("文件父级目录=" + file.getParent());
System.out.println("文件大小(字节)=" + file.length());
System.out.println("文件是否存在=" + file.exists());//T
System.out.println("是不是一个文件=" + file.isFile());//T
System.out.println("是不是一个目录=" + file.isDirectory());//F
韩顺平循序渐进学Java 零基础
第822页
}
}
```

流的分类

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-11-10-54-25-image.png" title="" alt="" width="527">

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-11-10-54-46-image.png" title="" alt="" width="533">

### FileInputStream使用

```java
//字节输入流，一次性读入4个字节的数据
public static void main(String[] args) throws IOException {
        String filepath = "E:\\test.txt";
        FileInputStream fis = new FileInputStream(filepath);
        byte[] buf =  new byte[4];
        int res = 0;
        while((res = fis.read(buf)) != -1) {
            System.out.print(new String(buf,0, res));
        }
        fis.close();
    }
//字节输入流，一次读入一个字节的数据
```

### FileOutputStream使用

```java
public class FileOutputStream01 {
public static void main(String[] args) {
}
/**
* 演示使用FileOutputStream 将数据写到文件中,
* 如果该文件不存在，则创建该文件
*/
@Test
韩顺平循序渐进学Java 零基础
第829页
public void writeFile() {
//创建 FileOutputStream 对象
String filePath = "e:\\a.txt";
FileOutputStream fileOutputStream = null;
try {
//得到 FileOutputStream 对象 对象
//老师说明
//1. new FileOutputStream(filePath) 创建方式，当写入内容是，会覆盖原来的内容
//2. new FileOutputStream(filePath, true) 创建方式，当写入内容是，是追加到文件后面
fileOutputStream = new FileOutputStream(filePath, true);
//写入一个字节
//fileOutputStream.write('H');//
//写入字符串
String str = "hsp,world!";
//str.getBytes() 可以把 字符串-> 字节数组
//fileOutputStream.write(str.getBytes());
/*
write(byte[] b, int off, int len) 将 len 字节从位于偏移量 off 的指定字节数组写入此文件输出流
*/
fileOutputStream.write(str.getBytes(), 0, 3);
} catch (IOException e) {
e.printStackTrace();
} finally {
try {
fileOutputStream.close();
} catch (IOException e) {
e.printStackTrace();
}
}
}
}
```

### FileReader

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-11-11-17-21-image.png" title="" alt="" width="598">

```java
public class FileReader_ {
  public static void main(String[] args) {}
  /**
   * 单个字符读取文件
   */
  @Test
  public void readFile01() {
    String filePath = "e:\\story.txt";
    FileReader fileReader = null;
    int data = 0;
    //1. 创建FileReader 对象
    try {
      fileReader = new FileReader(filePath);
      //循环读取 使用read, 单个字符读取
      while ((data = fileReader.read()) != -1) {
        System.out.print((char) data);
      }
    } catch (IOException e) {
      e.printStackTrace();
    } finally {
      try {
        if (fileReader != null) {
          fileReader.close();
        }
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
  }
  /**
   * 字符数组读取文件
   */
  @Test
  public void readFile02() {
    System.out.println("~~~readFile02 ~~~");
    String filePath = "e:\\story.txt";
    FileReader fileReader = null;
    int readLen = 0;
    char[] buf = new char[8];
    //1. 创建FileReader 对象
    try {
      fileReader = new FileReader(filePath);
      //循环读取 使用read(buf), 返回的是实际读取到的字符数
      //如果返回-1, 说明到文件结束
      while ((readLen = fileReader.read(buf)) != -1) {
        System.out.print(new String(buf, 0, readLen));
      }
    } catch (IOException e) {
      e.printStackTrace();
    } finally {
      try {
        if (fileReader != null) {
          fileReader.close();
        }
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
  }
}
```

### FileWriter

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-11-11-18-03-image.png" title="" alt="" width="605">

```java
public class FileWriter_ {
  public static void main(String[] args) {
    String filePath = "e:\\note.txt";
    //创建FileWriter 对象
    FileWriter fileWriter = null;
    char[] chars = {
      'a',
      'b',
      'c'
    };
    try {
      fileWriter = new FileWriter(filePath); //默认是覆盖写入
      // 3) write(int):写入单个字符
      fileWriter.write('H');
      // 4) write(char[]):写入指定数组
      fileWriter.write(chars);
      // 5) write(char[],off,len):写入指定数组的指定部分
      fileWriter.write("韩顺平教育".toCharArray(), 0, 3);
      // 6) write（string）：写入整个字符串
      fileWriter.write(" 你好北京~");
      fileWriter.write("风雨之后，定见彩虹");
      // 7) write(string,off,len):写入字符串的指定部分
      fileWriter.write("上海天津", 0, 2);
      //在数据量大的情况下，可以使用循环操作.
    } catch (IOException e) {
      e.printStackTrace();
    } finally {
      //对应FileWriter , 一定要关闭流，或者flush 才能真正的把数据写入到文件
      //老韩看源码就知道原因.
      /*
      看看代码
      private void writeBytes() throws IOException {
      this.bb.flip();
      int var1 = this.bb.limit();
      int var2 = this.bb.position();
      assert var2 <= var1;
      int var3 = var2 <= var1 ? var1 - var2 : 0;
      if (var3 > 0) {
      if (this.ch != null) {
      assert this.ch.write(this.bb) == var3 : var3;
      } else {
      this.out.write(this.bb.array(), this.bb.arrayOffset() + var2, var3);
      }
      }
      this.bb.clear();
      }
      */
      try {
        //fileWriter.flush();
        //关闭文件流，等价 flush() + 关闭
        fileWriter.close();
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
    System.out.println("程序结束...");
  }
}
```

### 节点流和处理流

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-11-11-25-59-image.png" title="" alt="" width="566">

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-11-11-27-39-image.png" title="" alt="" width="569">

节点流和处理流的区别和联系

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-11-11-28-14-image.png" title="" alt="" width="583">

处理流的功能主要提现在以下两个方面

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-11-11-31-22-image.png" title="" alt="" width="626">

bufferedreader和bufferedwriter属于字符流，按照字符来读取数据

#### BufferedReader和BufferedWriter

```java
public class BufferedReader_ {
  public static void main(String[] args) throws Exception {
    String filePath = "e:\\a.java";
    //创建bufferedReader
    BufferedReader bufferedReader = new BufferedReader(new FileReader(filePath));
    韩顺平循序渐进学Java 零基础
    第842页
    //读取
    String line; //按行读取, 效率高
    //说明
    //1. bufferedReader.readLine() 是按行读取文件
    //2. 当返回null 时，表示文件读取完毕
    while ((line = bufferedReader.readLine()) != null) {
      System.out.println(line);
    }
    //关闭流, 这里注意，只需要关闭 BufferedReader ，因为底层会自动的去关闭 节点流
    //FileReader。
    /*
    public void close() throws IOException {
    synchronized (lock) {
    if (in == null)
    return;
    try {
    in.close();//in 就是我们传入的 new FileReader(filePath), 关闭了.
    } finally {
    in = null;
    cb = null;
    }
    }
    }
    */
    bufferedReader.close();
  }
}
```

```java
public class BufferedCopy_ {
  public static void main(String[] args) {
    //老韩说明
    //1. BufferedReader 和 BufferedWriter 是安装字符操作
    //2. 不要去操作 二进制文件[声音，视频，doc, pdf ], 可能造成文件损坏
    //BufferedInputStream
    //BufferedOutputStream
    String srcFilePath = "e:\\a.java";
    String destFilePath = "e:\\a2.java";
    // String srcFilePath = "e:\\0245_韩顺平零基础学Java_引出this.avi";
    // String destFilePath = "e:\\a2 韩顺平.avi";
    BufferedReader br = null;
    BufferedWriter bw = null;
    String line;
    try {
      br = new BufferedReader(new FileReader(srcFilePath));
      bw = new BufferedWriter(new FileWriter(destFilePath));
      //说明: readLine 读取一行内容，但是没有换行
      while ((line = br.readLine()) != null) {
        //每读取一行，就写入
        bw.write(line);
        //插入一个换行
        bw.newLine();
      }
      System.out.println("拷贝完毕...");
    } catch (IOException e) {
      e.printStackTrace();
    } finally {
      //关闭流
      try {
        if (br != null) {
          br.close();
        }
        if (bw != null) {
          bw.close();
        }
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
  }
}
```

BufferedReader读取一行的时候不包括换行符，因此复制操作时，需要使用newLine手动插入换行

#### BufferedInputStream和BufferedOutputStream

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-11-12-29-44-image.png" title="" alt="" width="297">

具体方法和InputStream和OutputStream类似

#### 对象流ObjectInputStream和ObjectOutputStream

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-11-12-34-05-image.png" title="" alt="" width="574">

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-11-12-35-21-image.png" title="" alt="" width="538">

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-11-12-35-54-image.png" title="" alt="" width="529">

ObjectOutputStream的使用

```java
public static void main(String[] args) throws IOException {
        String filepath = "E:\\test.txt";
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(filepath));
        oos.writeUTF("this");
        oos.writeInt(12);
        oos.writeUTF("name");
        oos.close();
    }
```

ObjectInputStream的使用

```java
public static void main(String[] args) throws IOException {
        String filepath = "E:\\test.txt";
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream(filepath));
        System.out.println(ois.readUTF());
        System.out.println(ois.readInt());
        System.out.println(ois.readUTF());
    }
```

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-11-18-50-17-image.png" title="" alt="" width="584">

### 标准输入输出流

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-11-18-51-44-image.png" title="" alt="" width="452">

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-11-18-52-00-image.png" title="" alt="" width="424">

### 转换流InputStreamReader和OutputStreamWriter

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-11-18-59-16-image.png" title="" alt="" width="570">

```java
public static void main(String[] args) throws IOException {
  String filePath = "e:\\a.txt";
  //解读
  //1. 把 FileInputStream 转成 InputStreamReader
  //2. 指定编码 gbk
  //InputStreamReader isr = new InputStreamReader(new FileInputStream(filePath), "gbk");
  //3. 把 InputStreamReader 传入 BufferedReader
  //BufferedReader br = new BufferedReader(isr);
  //将2 和 3 合在一起
  BufferedReader br = new BufferedReader(new InputStreamReader(
    new FileInputStream(filePath), "gbk"));
  //4. 读取
  String s = br.readLine();
  System.out.println("读取内容=" + s);
  //5. 关闭外层流
  br.close();
}
```

## 反射机制

### 动态代理

可以在不修改代码的基础上为程序添加新的功能

<img title="" src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-16-10-45-14-image.png" alt="" width="584">

<img title="" src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-16-10-47-27-image.png" alt="" width="354">

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-16-10-59-32-image.png" title="" alt="" width="605">

代码示例

BigStar类

```java
package com.demo3;

public class BigStar implements Star{
    private String name;

    //唱、跳、rap
    @Override
    public String sing(String song) {
        System.out.println("我" + this.name + "要唱" + song + "……");
        return "thank you";
    }

    @Override
    public void dance() {
        System.out.println(this.name + "跳舞");
    }

    public BigStar() {
    }

    public BigStar(String name) {
        this.name = name;
    }

    /**
     * 获取
     * @return name
     */
    public String getName() {
        return name;
    }

    /**
     * 设置
     * @param name
     */
    public void setName(String name) {
        this.name = name;
    }

    public String toString() {
        return "BigStar{name = " + name + "}";
    }
}
```

Star接口，规范代理类和要代理的对象的方法

```java
package com.demo3;

public interface Star {

    //把所有要代理的方法写在接口中
    String sing(String name);
    void dance();
}
```

代理类

```java
package com.demo3;

import org.junit.Test;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class ProxyUtil {

    @Test
    public void test() {
        BigStar bigStar = new BigStar("鸡哥");
        Star proxy = ProxyUtil.createProxy(bigStar);
        String result = proxy.sing("鸡你太美！！！");
        System.out.println(result);
    }

    //该类用来创建一个代理对象，形参为被代理的明星对象，返回值为给明星创建的代理
    public static Star createProxy(BigStar bigStar) {
        Star star = (Star)Proxy.newProxyInstance(
                ProxyUtil.class.getClassLoader(),//指定用哪个类加载器，去加载生成的代理类
                new Class[]{Star.class}, //指定接口，这些接口用于指定生成的代理长什么样，也就是有哪些方法
                new InvocationHandler() { //指定生成的代理要干什么事情
                    @Override
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                        /*
                        * 参数一：代理的对象，一般不用，
                        * 参数二：要运行的方法，sing、dance等，
                        * 参数三：调用sing方法时，传递的实参
                        * */
                        //准备工作
                        if("sing".equals(method.getName())) {
                            System.out.println("准备话筒，收钱");
                        }else if("dance".equals(method.getName())) {
                            System.out.println("准备场地，收钱");
                        }
                        //找明星开始干活
                        return method.invoke(bigStar, args);
                    }
                }
        );

        return star;
    }
}
```

### 反射

反射允许对成员变量，成员方法和构造方法的信息进行编程访问

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-16-14-36-56-image.png" title="" alt="" width="608">

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-16-11-36-23-image.png" title="" alt="" width="606">

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-16-14-37-25-image.png" title="" alt="" width="595">

```java
public class ReflectDemo {

    public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException, IllegalAccessException, InvocationTargetException, InstantiationException {
        //反射demo
        Class c1 = Class.forName("com.demo3.BigStar");
        //获得无参构造
        Constructor<?> con1 = c1.getConstructor();
        //通过无参构造创建对象，返回的是Object类型，需要进行强制类型转换
        BigStar b1 = (BigStar) con1.newInstance();
        System.out.println(b1);
        //获得有参构造器
        Constructor<?> con2 = c1.getConstructor(String.class);
        BigStar b2 = (BigStar) con2.newInstance("鸡哥");
        System.out.println(b2);

        //获得所有的public修饰的构造方法
        Constructor[] cs1 = c1.getConstructors();
        for(Constructor c: cs1) {
            System.out.println(c);
        }
        System.out.println("------");
        //获得所有构造方法，包括私有
        Constructor[] cs2 = c1.getDeclaredConstructors();
        for(Constructor c: cs2) {
            System.out.println(c);
            //临时取消权限校验，可以使用私有构造方法创建对象
            c.setAccessible(true);
            System.out.println(c.getModifiers());
        }
        System.out.println("----");
        //获取构造方法的权限修饰符
        int modifiers = con2.getModifiers();
        System.out.println(modifiers);
        //获取所有参数
        Parameter[] p1s = con2.getParameters();
        for(Parameter p: p1s) {
            System.out.println(p);
        }
    }
}
```

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-16-14-58-15-image.png" title="" alt="" width="445">

<img title="" src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-16-15-01-29-image.png" alt="" width="378" data-align="inline">

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-16-15-02-17-image.png" title="" alt="" width="539">

通过getMethods获得的所有方法对象包含父类中所有的公共方法

通过getDeclaredMethods获得子类的所有方法，包括私有方法，不包含父类的方法

<img src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-16-15-06-54-image.png" title="" alt="" width="694">

<img title="" src="file:///C:/Users/29579/AppData/Roaming/marktext/images/2024-01-16-15-09-22-image.png" alt="" width="402">

