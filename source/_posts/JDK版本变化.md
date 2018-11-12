---
title: JDK版本变化
date: 2018-10-23 16:12:48
categories: "JDK"
comments: true
tags:
- JDK 
- Java
---

## 简介
记录一下JDK版本的历史更新和重要特性。

目前JDK更新较快，每半年发布一个版本，但是对于生产环境采用仍然建议使用LTS(长期支持版本)版本

按照 Oracle 公布的支持路线图，Java 11 将会获得 Oracle 提供的长期支持服务，直至2026年9月。
![](https://raw.githubusercontent.com/victoryofymk/victoryofymk.github.io/images/JDK%20%E7%89%88%E6%9C%AC%E5%8F%98%E5%8C%96-1.png)

根据官网从Java 11开始提供用开源许可证和商业许可证的组合


### 更新记录
* 2018-11-09 补充JDK8的介绍和使用


## 一些术语
* JCP 是 Java Community Process（Java社区进程）的简称，社会各界Java组成的社区，规划和领导Java的发展。
* JEP 是 JDK Enhancement Proposals （Java 增强提案）的简称，JDK的版本变化将从这些提案中选取。
* JSR 是 Java Specification Requests（Java规范请求）的简称，是 JCP 成员向委员会提交的 Java 发展议案，经过一系列流程后，如果通过会成为 JEP，最终会体现在未来的Java中。
* TCK 是 Technology Compatibility Kit（技术兼容性测试）的简称， 如果一个平台型程序想要宣称自己兼容Java，就必须通过TCK测试

## JDK6
* 集合框架增强。
1. 为了更好的支持双向访问集合。添加了许多新的类和接口。
2. 新的数组拷贝方法。Arrays.copyOf和Arrays.copyOfRange

```
//以下为添加的新接口和类
Deque,BlockingDeque,NavigableSet,NavigableMap,ConcurrentNavigableMap，ArrayDeque， ConcurrentSkipListSet ,ConcurrentSkipListMap,ConcurrentSkipListMap ,AbstractMap.SimpleEntry ,AbstractMap.SimpleImmutableEntry
```

* Scripting. 可以让其他语言在java平台上运行。 java6包含了一个基于Mozilla Rhino实现的javascript脚本引擎。
* 支持JDBC4.0规范。

## JDK7

* 二进制前缀0b或者0B。整型（byte, short, int, long）可以直接用二进制表示。
* 字面常量数字的下划线。用下划线连接整数提升其可读性，自身无含义，不可用在数字的起始和末尾。
* switch 支持String类型。
* 泛型实例化类型自动推断。
* try-with-resources语句。
* 单个catch中捕获多个异常类型（用| 分割）并通过改进的类型检查重新抛出异常。


## JDK8

* Lambda 表达式(Lambda Expressions) − Lambda允许把函数作为一个方法的参数（函数作为参数传递进方法中。
* 方法引用（Method references） − 方法引用提供了非常有用的语法，可以直接引用已有Java类或对象（实例）的方法或构造器。与lambda联合使用，方法引用可以使语言的构造更紧凑简洁，减少冗余代码。
* 默认方法 （Default methods）− 默认方法就是一个在接口里面有了一个实现的方法。
* 新工具 − 新的编译工具，如：Nashorn引擎 jjs、 类依赖分析器jdeps。
* Stream API −新添加的Stream API（java.util.stream） 把真正的函数式编程风格引入到Java中。
* Date Time API − 加强对日期与时间的处理。
* Optional 类 − Optional 类已经成为 Java 8 类库的一部分，用来解决空指针异常。
* Nashorn, JavaScript 引擎 − Java 8提供了一个新的Nashorn javascript引擎，它允许我们在JVM上运行特定的javascript应用。
* java.util 包下的改进，提供了几个实用的工具类。
  并行数组排序。
  标准的Base64编解码。
  支持无符号运算。

* java.util.concurrent 包下增加了新的类和方法。
  java.util.concurrent.ConcurrentHashMap 类添加了新的方法以支持新的StreamApi和lambada表达式。
  java.util.concurrent.atomic 包下新增了类以支持可伸缩可更新的变量。
  java.util.concurrent.ForkJoinPool类新增了方法以支持 common pool。
  新增了java.util.concurrent.locks.StampedLock类，为控制读/写访问提供了一个基于性能的锁，且有三种模式可供选择。
* HotSpot
  删除了 永久代（PermGen）.
  方法调用的字节码指令支持默认方法。

* 重复注解（Repeating Annotations）。重复注解提供了在同一声明或类型中多次应用相同注解类型的能力。
* 类型注解（Type Annotation）。在任何地方都能使用注解，而不是在声明的地方。
* 类型推断增强
* 方法参数反射（Method Parameter Reflection）。
* HashMap改进，在键值哈希冲突时能有更好表现。

部分特性说明：
### 1.Lambda 表达式
Lambda 表达式，也可称为闭包，允许把函数作为一个方法的参数
#### 语法
lambda 表达式的语法格式如下：
```
(parameters) -> expression
或
(parameters) ->{ statements; }
```
以下是lambda表达式的重要特征:

```
可选类型声明：不需要声明参数类型，编译器可以统一识别参数值。
可选的参数圆括号：一个参数无需定义圆括号，但多个参数需要定义圆括号。
可选的大括号：如果主体包含了一个语句，就不需要使用大括号。
可选的返回关键字：如果主体只有一个表达式返回值则编译器会自动返回值，大括号需要指定明表达式返回了一个数值。
```
Lambda 表达式实例

```
# 1. 不需要参数,返回值为 5  
() -> 5  
# 2. 接收一个参数(数字类型),返回其2倍的值  
x -> 2 * x  
# 3. 接受2个参数(数字),并返回他们的差值  
(x, y) -> x – y  
# 4. 接收2个int型整数,返回他们的和  
(int x, int y) -> x + y  
# 5. 接受一个 string 对象,并在控制台打印,不返回任何值(看起来像是返回void)  
(String s) -> System.out.print(s)
```
### 2.方法引用
方法引用通过方法的名字来指向一个方法,可以使语言的构造更紧凑简洁，减少冗余代码。方法引用使用一对冒号 ::
#### 语法
构造器引用：它的语法是Class::new，或者更一般的Class< T >::new实例如下：
```
final Car car = Car.create( Car::new );
final List< Car > cars = Arrays.asList( car );
```
静态方法引用：它的语法是Class::static_method，实例如下：
```
cars.forEach( Car::collide );
```
特定类的任意对象的方法引用：它的语法是Class::method实例如下：
```
cars.forEach( Car::repair );
```
特定对象的方法引用：它的语法是instance::method实例如下：
```
final Car police = Car.create( Car::new );
cars.forEach( police::follow );
```

方法引用实例

```
public class Java8Tester {
   public static void main(String args[]){
      List names = new ArrayList();
        
      names.add("Google");
      names.add("Runoob");
      names.add("Taobao");
      names.add("Baidu");
      names.add("Sina");
        
      names.forEach(System.out::println);
   }
}
```
### 函数式接口
函数式接口(Functional Interface)就是一个有且仅有一个抽象方法，但是可以有多个非抽象方法的接口。

函数式接口可以被隐式转换为 lambda 表达式。

如定义了一个函数式接口如下：
```
@FunctionalInterface
interface GreetingService 
{
    void sayMessage(String message);
}
```
那么就可以使用Lambda表达式来表示该接口的一个实现(注：JAVA 8 之前一般是用匿名类实现的)：
```
GreetingService greetService1 = message -> System.out.println("Hello " + message);
```
### 默认方法
Java 8 新增了接口的默认方法。
简单说，默认方法就是接口可以有实现方法，而且不需要实现类去实现其方法。
我们只需在方法名前面加个default关键字即可实现默认方法。

```
为什么要有这个特性？

首先，之前的接口是个双刃剑，好处是面向抽象而不是面向具体编程，缺陷是，当需要修改接口时候，需要修改全部实现该接口的类，目前的java 8之前的集合框架没有foreach方法，通常能想到的解决办法是在JDK里给相关的接口添加新的方法及实现。然而，对于已经发布的版本，是没法在给接口添加新方法的同时不影响已有的实现。所以引进的默认方法。他们的目的是为了解决接口的修改与现有的实现不兼容的问题。
```
#### 语法
默认方法语法格式如下：

```
public interface Vehicle {
   default void print(){
      System.out.println("我是一辆车!");
   }
}
```

多个默认方法
一个接口有默认方法，考虑这样的情况，一个类实现了多个接口，且这些接口有相同的默认方法，以下实例说明了这种情况的解决方法：

```
public interface Vehicle {
   default void print(){
      System.out.println("我是一辆车!");
   }
}
 
public interface FourWheeler {
   default void print(){
      System.out.println("我是一辆四轮车!");
   }
}
```
第一个解决方案是创建自己的默认方法，来覆盖重写接口的默认方法：

```
public class Car implements Vehicle, FourWheeler {
   default void print(){
      System.out.println("我是一辆四轮汽车!");
   }
}
```
第二种解决方案可以使用 super 来调用指定接口的默认方法：

```
public class Car implements Vehicle, FourWheeler {
   public void print(){
      Vehicle.super.print();
   }
}
```
静态默认方法
Java 8 的另一个特性是接口可以声明（并且可以提供实现）静态方法。例如：

```
public interface Vehicle {
   default void print(){
      System.out.println("我是一辆车!");
   }
    // 静态方法
   static void blowHorn(){
      System.out.println("按喇叭!!!");
   }
}
```
### Stream
Java 8 API添加了一个新的抽象称为流Stream，可以让你以一种声明的方式处理数据。

Stream 使用一种类似用 SQL 语句从数据库查询数据的直观方式来提供一种对 Java 集合运算和表达的高阶抽象。

Stream API可以极大提高Java程序员的生产力，让程序员写出高效率、干净、简洁的代码。

这种风格将要处理的元素集合看作一种流， 流在管道中传输， 并且可以在管道的节点上进行处理， 比如筛选， 排序，聚合等。

元素流在管道中经过中间操作（intermediate operation）的处理，最后由最终操作(terminal operation)得到前面处理的结果。

```
+--------------------+       +------+   +------+   +---+   +-------+
| stream of elements +-----> |filter+-> |sorted+-> |map+-> |collect|
+--------------------+       +------+   +------+   +---+   +-------+
```

以上的流程转换为 Java 代码为：

```
List<Integer> transactionsIds = 
widgets.stream()
             .filter(b -> b.getColor() == RED)
             .sorted((x,y) -> x.getWeight() - y.getWeight())
             .mapToInt(Widget::getWeight)
             .sum();
```
#### 什么是 Stream？
Stream（流）是一个来自数据源的元素队列并支持聚合操作

元素是特定类型的对象，形成一个队列。 Java中的Stream并不会存储元素，而是按需计算。
数据源 流的来源。 可以是集合，数组，I/O channel， 产生器generator 等。
聚合操作 类似SQL语句一样的操作， 比如filter, map, reduce, find, match, sorted等。
和以前的Collection操作不同， Stream操作还有两个基础的特征：

Pipelining: 中间操作都会返回流对象本身。 这样多个操作可以串联成一个管道， 如同流式风格（fluent style）。 这样做可以对操作进行优化， 比如延迟执行(laziness)和短路( short-circuiting)。
内部迭代： 以前对集合遍历都是通过Iterator或者For-Each的方式, 显式的在集合外部进行迭代， 这叫做外部迭代。 Stream提供了内部迭代的方式， 通过访问者模式(Visitor)实现。
#### 生成流
在 Java 8 中, 集合接口有两个方法来生成流：

stream() − 为集合创建串行流。

parallelStream() − 为集合创建并行流。

```
List<String> strings = Arrays.asList("abc", "", "bc", "efg", "abcd","", "jkl");
List<String> filtered = strings.stream().filter(string -> !string.isEmpty()).collect(Collectors.toList());
```
#### forEach
Stream 提供了新的方法 'forEach' 来迭代流中的每个数据。以下代码片段使用 forEach 输出了10个随机数：

```
Random random = new Random();
random.ints().limit(10).forEach(System.out::println);
```
#### map

map 方法用于映射每个元素到对应的结果，以下代码片段使用 map 输出了元素对应的平方数：

```
List<Integer> numbers = Arrays.asList(3, 2, 2, 3, 7, 3, 5);
// 获取对应的平方数
List<Integer> squaresList = numbers.stream().map( i -> i*i).distinct().collect(Collectors.toList());
```
#### filter
filter 方法用于通过设置的条件过滤出元素。以下代码片段使用 filter 方法过滤出空字符串：

```
List<String>strings = Arrays.asList("abc", "", "bc", "efg", "abcd","", "jkl");
// 获取空字符串的数量
int count = strings.stream().filter(string -> string.isEmpty()).count();
```
#### limit
limit 方法用于获取指定数量的流。 以下代码片段使用 limit 方法打印出 10 条数据：

```
Random random = new Random();
random.ints().limit(10).forEach(System.out::println);
```
#### sorted
sorted 方法用于对流进行排序。以下代码片段使用 sorted 方法对输出的 10 个随机数进行排序：

```
Random random = new Random();
random.ints().limit(10).sorted().forEach(System.out::println);
```
#### 并行（parallel）程序
parallelStream 是流并行处理程序的代替方法。以下实例我们使用 parallelStream 来输出空字符串的数量：

```
List<String> strings = Arrays.asList("abc", "", "bc", "efg", "abcd","", "jkl");
// 获取空字符串的数量
int count = strings.parallelStream().filter(string -> string.isEmpty()).count();
```
我们可以很容易的在顺序运行和并行直接切换。

#### Collectors
Collectors 类实现了很多归约操作，例如将流转换成集合和聚合元素。Collectors 可用于返回列表或字符串：

```
List<String>strings = Arrays.asList("abc", "", "bc", "efg", "abcd","", "jkl");
List<String> filtered = strings.stream().filter(string -> !string.isEmpty()).collect(Collectors.toList());
 
System.out.println("筛选列表: " + filtered);
String mergedString = strings.stream().filter(string -> !string.isEmpty()).collect(Collectors.joining(", "));
System.out.println("合并字符串: " + mergedString);
```
#### 统计
另外，一些产生统计结果的收集器也非常有用。它们主要用于int、double、long等基本类型上，它们可以用来产生类似如下的统计结果。

```
List<Integer> numbers = Arrays.asList(3, 2, 2, 3, 7, 3, 5);
 
IntSummaryStatistics stats = numbers.stream().mapToInt((x) -> x).summaryStatistics();
 
System.out.println("列表中最大的数 : " + stats.getMax());
System.out.println("列表中最小的数 : " + stats.getMin());
System.out.println("所有数之和 : " + stats.getSum());
System.out.println("平均数 : " + stats.getAverage());
```
### Optional 类
Optional 类是一个可以为null的容器对象。如果值存在则isPresent()方法会返回true，调用get()方法会返回该对象。

Optional 是个容器：它可以保存类型T的值，或者仅仅保存null。Optional提供很多有用的方法，这样我们就不用显式进行空值检测。

Optional 类的引入很好的解决空指针异常。

类声明
以下是一个 java.util.Optional<T> 类的声明：

```
public final class Optional<T>
extends Object
```
### Nashorn JavaScript
Nashorn 一个 javascript 引擎。

从JDK 1.8开始，Nashorn取代Rhino(JDK 1.6, JDK1.7)成为Java的嵌入式JavaScript引擎。Nashorn完全支持ECMAScript 5.1规范以及一些扩展。它使用基于JSR 292的新语言特性，其中包含在JDK 7中引入的 invokedynamic，将JavaScript编译成Java字节码。

与先前的Rhino实现相比，这带来了2到10倍的性能提升。

### 日期时间 API
ava 8通过发布新的Date-Time API (JSR 310)来进一步加强对日期与时间的处理。

在旧版的 Java 中，日期时间 API 存在诸多问题，其中有：

非线程安全 − java.util.Date 是非线程安全的，所有的日期类都是可变的，这是Java日期类最大的问题之一。

设计很差 − Java的日期/时间类的定义并不一致，在java.util和java.sql的包中都有日期类，此外用于格式化和解析的类在java.text包中定义。java.util.Date同时包含日期和时间，而java.sql.Date仅包含日期，将其纳入java.sql包并不合理。另外这两个类都有相同的名字，这本身就是一个非常糟糕的设计。

时区处理麻烦 − 日期类并不提供国际化，没有时区支持，因此Java引入了java.util.Calendar和java.util.TimeZone类，但他们同样存在上述所有的问题。

Java 8 在 java.time 包下提供了很多新的 API。以下为两个比较重要的 API：

Local(本地) − 简化了日期时间的处理，没有时区的问题。

Zoned(时区) − 通过制定的时区处理日期时间。

新的java.time包涵盖了所有处理日期，时间，日期/时间，时区，时刻（instants），过程（during）与时钟（clock）的操作。

###  Base64
在Java 8中，Base64编码已经成为Java类库的标准。

Java 8 内置了 Base64 编码的编码器和解码器。

Base64工具类提供了一套静态方法获取下面三种BASE64编解码器：

基本：输出被映射到一组字符A-Za-z0-9+/，编码不添加任何行标，输出的解码仅支持A-Za-z0-9+/。
URL：输出映射到一组字符A-Za-z0-9+_，输出是URL和文件。
MIME：输出隐射到MIME友好格式。输出每行不超过76字符，并且使用'\r'并跟随'\n'作为分割。编码输出最后没有行分割。
## JDK9
JDK 9 于2017年9月21日发布

新特性有：

* 模块化 —— Jigsaw（Java Platform Module System）
* 交互式命令行 —— JShell
* 进程操作改进
* 竞争锁的性能优化
* 分段代码缓存
* 优化字符串占用空间
* Javadoc
  简化Doclet API。
  支持生成HTML5格式。
  加入了搜索框,使用这个搜索框可以查询程序元素、标记的单词和文档中的短语。
  支持新的模块系统。
* JVM
  增强了Garbage-First(G1)并用它替代Parallel GC成为默认的垃圾收集器。
  统一了JVM 日志，为所有组件引入了同一个日志系统。
  删除了JDK 8中弃用的GC组合。（DefNew + CMS，ParNew + SerialOld，Incremental CMS）。
* properties文件支持UTF-8编码,之前只支持ISO-8859-1。
* 支持Unicode 8.0，在JDK8中是Unicode 6.2。
* 支持私有接口方法(您可以使用diamond语法与匿名内部类结合使用)。
* 下划线不能用在变量名中。
* diamond语法与匿名内部类结合使用。
* 新的版本号格式。$MAJOR.$MINOR.$SECURITY.$PATCH

## JDK10
北京时间 3 月 21 日，Oracle 官方宣布 Java 10 正式发布。新特性有：

* JEP 286: 局部变量的类型推导。该特性在社区讨论了很久并做了调查，可查看 JEP 286 调查结果。
* JEP 296: 将 JDK 的多个代码仓库合并到一个储存库中。
* JEP 304: 垃圾收集器接口。通过引入一个干净的垃圾收集器（GC）接口，改善不同垃圾收集器的源码隔离性。
* JEP 307: 向 G1 引入并行 Full GC。
* JEP 310: 应用类数据共享。为改善启动和占用空间，在现有的类数据共享（“CDS”）功能上再次拓展，以允许应用类放置在共享存档中。
* JEP 312: 线程局部管控。允许停止单个线程，而不是只能启用或停止所有线程。
* JEP 313: 移除 Native-Header Generation Tool (javah)
* JEP 314: 额外的 Unicode 语言标签扩展。包括：cu (货币类型)、fw (每周第一天为星期几)、rg (区域覆盖)、tz (时区) 等。
* JEP 316: 在备用内存设备上分配堆内存。允许 HotSpot 虚拟机在备用内存设备上分配 Java 对象堆。
* JEP 317: 基于 Java 的 JIT 编译器（试验版本）。
* JEP 319: 根证书。开源 Java SE Root CA 程序中的根证书。
* JEP 322: 基于时间的版本发布模式。“Feature releases” 版本将包含新特性，“Update releases” 版本仅修复 Bug 。

部分特性说明

```
1. var 类型推断。

这个语言功能在其他一些语言 (C#、JavaScript) 和基于 JRE 的一些语言 (Scala 和 Kotlin) 中，早已被加入。

在 Java 语言很早就在考虑，早在 2016 年正式提交了 JEP286 提议。后来举行了一次公开的开发者调查，获得最多建议的是采用类似 Scala 的方案，“同时使用 val 和 var”，约占一半；第二多的是“只使用 var”，约占四分之一。后来 Oracle 公司经过慎重考虑，采用了只使用 var 关键字的方案。

有了这个功能，开发者在写这样的代码时：

ArrayList<String> myList = new ArrayList<String>()
可以省去前面的类型声明，而只需要

var list = new ArrayList<String>()
编译器会自动推断出 list 变量的类型。对于链式表达式来说，也会很方便：

var stream = blocks.stream(); 

... 

int maxWeight = stream.filter(b -> b.getColor() == BLUE)

                      .mapToInt(Block::getWeight)

                      .max();
开发者无须声明并且 import 引入 Stream 类型，只用 stream 作为中间变量，用 var 关键字使得开发效率提升。

不过 var 的使用有众多限制，包括不能用于推断方法参数类型，只能用于局部变量，如方法块中，而不能用于类变量的声明，等等。

另外，我个人认为，对于开发者而言，变量类型明显的声明会提供更加全面的程序语言信息，对于理解并维护代码有很大的帮助。一旦 var 被广泛运用，开发者阅读三方代码而没有 IDE 的支持下，会对程序的流程执行理解造成一定的障碍。所以我建议尽量写清楚变量类型，程序的易读维护性有时更重要一些。

2. 统一的 GC 接口

在 JDK10 的代码中，路径为 openjdk/src/hotspot/share/gc/，各个 GC 实现共享依赖 shared 代码，GC 包括目前默认的 G1，也有经典的 Serial、Parallel、CMS 等 GC 实现。



3. 应用程序类数据（AppCDS）共享

CDS 特性在原来的 bootstrap 类基础之上，扩展加入了应用类的 CDS(Application Class-Data Sharing) 支持。

其原理为：在启动时记录加载类的过程，写入到文本文件中，再次启动时直接读取此启动文本并加载。设想如果应用环境没有大的变化，启动速度就会得到提升。

我们可以想像为类似于操作系统的休眠过程，合上电脑时把当前应用环境写入磁盘，再次使用时就可以快速恢复环境。

我在自己 PC 电脑上做以下应用启动实验。

首先部署 wildfly 12 应用服务器，采用 JDK10 预览版作为 Java 环境。另外需要用到一个工具 cl4cds[1]，作用是把加载类的日志记录，转换为 AppCDS 可以识别的格式。

A、安装好 wildfly 并部署一个应用，具有 Angularjs, rest, jpa 完整应用技术栈，预热后启动三次，并记录完成部署时间

分别为 6716ms, 6702ms, 6613ms，平均时间为 6677ms。

B、加入环境变量并启动，导出启动类日志

export PREPEND_JAVA_OPTS="-Xlog:class+load=debug:file=/tmp/wildfly.classtrace"
C、使用 cl4cds 工具，生成 AppCDS 可以识别的 cls 格式

/jdk-10/bin/java -cp src/classes/ io.simonis.cl4cds /tmp/wildfly.classtrace /tmp/wildfly.cls
打开文件可以看到内容为：

java/lang/Object id: 0x0000000100000eb0

java/io/Serializable id: 0x0000000100001090

java/lang/Comparable id: 0x0000000100001268

java/lang/CharSequence id: 0x0000000100001440

......

org/hibernate/type/AssociationType id: 0x0000000100c61208 super: 0x0000000100000eb0 interfaces: 0x0000000100a00d10 source: /home/shihang/work/jboss/wildfly/dist/target/wildfly-12.0.0.Final/modules/system/layers/base/org/hibernate/main/hibernate-core-5.1.10.Final.jar

org/hibernate/type/AbstractType id: 0x0000000100c613e0 super: 0x0000000100000eb0 interfaces: 0x0000000100a00d10 source: /home/shihang/work/jboss/wildfly/dist/target/wildfly-12.0.0.Final/modules/system/layers/base/org/hibernate/main/hibernate-core-5.1.10.Final.jar

org/hibernate/type/AnyType id: 0x0000000100c61820 super: 0x0000000100c613e0 interfaces: 0x0000000100c61030 0x0000000100c61208 source: /home/shihang/work/jboss/wildfly/dist/target/wildfly-12.0.0.Final/modules/system/layers/base/org/hibernate/main/hibernate-core-5.1.10.Final.jar

....
这个文件用于标记类的加载信息。

D、使用环境变量启动 wildfly，模拟启动过程并导出 jsa 文件，就是记录了启动时类的信息。

export PREPEND_JAVA_OPTS="-Xshare:dump -XX:+UseAppCDS -XX:SharedClassListFile=/tmp/wildfly.cls -XX:+UnlockDiagnosticVMOptions -XX:SharedArchiveFile=/tmp/wildfly.jsa"
查看产生的文件信息，jsa 文件有较大的体积。

/opt/work/cl4cds$ ls -l /tmp/wildfly.*

-rw-rw-r-- 1 shihang shihang   8413843 Mar 20 11:07 /tmp/wildfly.classtrace

-rw-rw-r-- 1 shihang shihang   4132654 Mar 20 11:11 /tmp/wildfly.cls

-r--r--r-- 1 shihang shihang 177659904 Mar 20 11:13 /tmp/wildfly.jsa
E、使用 jsa 文件启动应用服务器

export PREPEND_JAVA_OPTS="-Xshare:on -XX:+UseAppCDS -XX:+UnlockDiagnosticVMOptions -XX:SharedArchiveFile=/tmp/wildfly.jsa"
启动完毕后记录时长，三次分别是 5535ms, 5333ms, 5225ms，平均为 5364ms，相比之前的 6677ms 可以算出启动时间提升了 20% 左右。

这个效率提升，对于云端应用部署很有价值。

以上实验方法参考于技术博客 [2]。

4. JEP314，使用附加的 Unicode 语言标记扩展。

JDK10 对于 Unicode BCP 47 有了更多的支持，BCP 47 是 IETF 定义语言集的规范文档。使用扩展标记，可以更方便的获得所需要的语言地域环境。

如 JDK10 加入的一个方法，

java.time.format.DateTimeFormatter::localizedBy
通过这个方法，可以采用某种数字样式，区域定义或者时区来获得时间信息所需的语言地域本地环境信息。

附：从链接 [3] 可以看到 JDK10 所有的方法级别改动。

5. 查看当前 JDK 管理根证书。

自 JDK9 起在 keytool 中加入参数 -cacerts，可以查看当前 JDK 管理的根证书。而 OpenJDK9 中 cacerts 为空，这样就会给开发者带来很多不变。

EP318 就是利用 Oracle 开源出 Oracle JavaSE 中的 cacerts 信息，在 OpenJDK 中提供一组默认的根证书颁发机构证书，目前有 80 条记录。

/jdk-10/bin$ ./keytool -list -cacerts

Enter keystore password:  

Keystore type: JKS

Keystore provider: SUN



Your keystore contains 80 entries



verisignclass2g2ca [jdk], Dec 2, 2017, trustedCertEntry, 

Certificate fingerprint (SHA-256): 3A:43:E2:20:FE:7F:3E:A9:65:3D:1E:21:74:2E:AC:2B:75:C2:0F:D8:98:03:05:BC:50:2C:AF:8C:2D:9B:41:A1

......
```
## JDK11

JDK 11 总共包含 17 个新的 JEP ，分别为：

* 181: Nest-Based Access Control（基于嵌套的访问控制）
* 309: Dynamic Class-File Constants（动态类文件常量）
* 315: Improve Aarch64 Intrinsics（改进 Aarch64 Intrinsics）
* 318: Epsilon: A No-Op Garbage Collector（Epsilon — 一个无操作的垃圾收集器）
* 320: Remove the Java EE and CORBA Modules（删除 Java EE 和 CORBA 模块）
* 321: HTTP Client (Standard)
* 323: Local-Variable Syntax for Lambda Parameters（用于 Lambda 参数的局部变量语法）
* 324: Key Agreement with Curve25519 and Curve448（Curve25519 和 Curve448 算法的密钥协议）
* 327: Unicode 10
* 328: Flight Recorder
* 329: ChaCha20 and Poly1305 Cryptographic Algorithms（ChaCha20 和 Poly1305 加密算法）
* 330: Launch Single-File Source-Code Programs（启动单一文件的源代码程序）
* 331: Low-Overhead Heap Profiling（低开销的 Heap Profiling）
* 332: Transport Layer Security (TLS) 1.3（支持 TLS 1.3）
* 333: ZGC: A Scalable Low-Latency Garbage Collector (Experimental) （可伸缩低延迟垃圾收集器）
* 335: Deprecate the Nashorn JavaScript Engine（弃用 Nashorn JavaScript 引擎）
* 336: Deprecate the Pack200 Tools and API （弃用 Pack200 工具和 API）

支持Unicode 10.0,在jdk10中是8.0。
标准化HTTP Client
编译器线程的延迟分配。添加了新的命令-XX:+UseDynamicNumberOfCompilerThreads动态控制编译器线程的数量。
新的垃圾收集器—ZGC。一种可伸缩的低延迟垃圾收集器(实验性)。
Epsilon。一款新的实验性无操作垃圾收集器。Epsilon GC 只负责内存分配，不实现任何内存回收机制。这对于性能测试非常有用，可用于与其他GC对比成本和收益。
Lambda参数的局部变量语法。java10中引入的var字段得到了增强，现在可以用在lambda表达式的声明中。如果lambda表达式的其中一个形式参数使用了var，那所有的参数都必须使用var。


## JDK12


## 参考资料
1.[JDK 版本变化](https://www.jianshu.com/p/31433bcaa1a5)
2.[Java 9 揭秘](https://www.cnblogs.com/IcanFixIt/p/7131676.html)


