---
title: Java 期末速通手册
description: 尝试拯救 WHUer 的 Java 期末考试
slug: java-speedrun-doc
date: 2026-01-08 13:18:03+0000
image: cover.jpg
categories:
    - Learning
tags:
    - Java
    - Programming
weight: 1
---

这个手册旨在帮助你在尽可能短的时间内达到《面向对象程序设计》这门课的及格要求。

如果你想要冲刺满绩，这本手册只能作为你的辅助资料。

本手册假定读者：

* 是人类，能读懂自然语言；
* 有一点点编程的基础，至少能看得懂一点代码；
* 愿意花点时间学。

让我们直接按 PPT 的顺序开始。

手册的后续部分对章节顺序做了一些调整，以帮助读者尽可能以**沉浸的、连续式的**阅读体验来进行速通。

**本手册不是任何专业性质的编程指南或编程规范！**

本手册由 Rhapsody0x1 于2026 年 1 月撰写于珞珈山下。

由于笔者水平有限，如果你发现了错误，请联系笔者 QQ 1227637059 进行修正，感激不尽！

**本指南免费分享，禁止用于包括倒卖在内的任何盈利行为！**

如果你通过付费渠道获得了本指南，那么你被骗了。

## 目录

[TOC]

## Java 起步入门

这一章的内容不算太多，主要讲了 Java 历史和一些特点。

如果你只学过课内的一点 C++ ，不了解别的编程语言的话还是有必要了解一下。

### Java 是什么语言

PPT 上叽里咕噜叭啦了很多，这里笔者把自认为比较重要的摘出来：

> Java 是**简单的**、**面向对象的**、**解释型**、**平台独立的**编程语言。

然后我们一条条的来说，主要跟 C 和 C++ 对照一下。

#### 为啥简单？

Java 删掉了 C++ 里一堆乱七八糟的特性：

* 指针 ——包装成**引用**了，这个我们后面着重说；

* 多重继承；

* 运算符重载；

> 注：String的 `+` 是 Java 内建的，**不是**用户定义的运算符重载。你不能为自己的类重载运算符。

* 手动内存管理 ——Java是一个典型的**有GC**语言，内存可以自动管理；

* 语法也更规整，写起来不容易踩坑。

当然，Java 自己也有自己的坑，比如 `==` 和 `equals` 的[差别](###Object 类)，`int` 和 `Integer` 的[区别](###基本类型的包装类)，这个我们后面再说。

#### 啥是面向对象？

面向对象 (OO，Object-Oriented) 就是把现实世界的东西抽象成**对象**，每个对象既有数据（在 Java 里叫**字段**或**属性**），也有行为（叫**方法**或者**函数**）。

Java 里除了基本数据类型之外**一切皆对象**。**封装**、**多态**、**继承**、**抽象**在 Java 里全部都有，我们放到[后面](##面向对象特征)再细说。

#### 解释型的？

有一个常见的划分方法是，将 Java 和 C++ 放到一起称为编译型语言，将 Python 和 JS 放到一起称为解释型语言。但 Java 是比较特别的，Java 代码**不是**直接编译成机器码，而是先编译成**字节码**，也就是 `.class` 文件里的内容，然后由 **JVM** 在运行时**解释执行**（这其中可能还涉及到复杂的 JIT 编译，感兴趣自己搜索了解，gh 老师班上的同学应该是听过这个的）。

#### 平台独立的？

只要我们有编译好的**字节码**，我们只需要为不同的平台（比如 Windows，macOS，Linux，甚至一些嵌入式设备）编写一次 JVM，就可以让这个字节码对应的 Java 程序到处运行。也就是常说的**一次编写，到处运行**。

### 我们用什么来写 Java

这一部分主要聊 Java 的开发环境搭建，本手册的读者应该都是急着顺利通过期末但是**至少写了作业**的，所以我们大胆~~(其实是懒狗)~~地跳过这一部分。

事实上 Java 的开发环境配置比 C++ 简单不少，如果你要追求高分的话还是自己多写一写。本手册中也有大量的例子，你可以复制代码到编辑器里，然后自己看一下编译器的报错。

### JShell

一个命令行里的交互式 Java 环境，我猜这玩意不考。

---

## 数据类型与运算符

在 C++ 里我们已经学了很多数据类型的知识，但在 Java 这里你会看到一些**不一样**的东西和独特的坑点，我们先从最简单的开始。

### 基本类型与引用类型

我们前面说过了 Java **万物皆对象**，那有没有例外呢，有！但是**仅限**下面的基本数据类型：

* **基本类型：`byte short int long float double char boolean`，**就这八种**，它们有各自的**包装类**，后面会提到；
* **引用类型**：除了上面 8 个以外**全部都是**，比如 `String`、数组、各种类对象，也包括**记录** (Record) 和**枚举** (Enum)。

简单来说，这俩最大的区别是：

* 基本类型变量里放的是**值本体**；
  * 比如 `int x = 3;` 的 3。
* 引用类型变量里放的是**地址/引用**；
  * 比如 `Employee e = new Employee();`，`e` 是对后面 `new` 产生的对象的**引用**，有点像 C++ 的指针，但是没那么难。注意，我们在很多场景下不会特别区分这里的 `e` 是“引用”还是“对象”，比如我们可能说“调用对象e的xxx函数”，但你**需要知道**我们其实是通过**引用**找到了对象后，调用了对象的xxx函数。

下面的表格呈现了一些别的区别，不过最重要的还是上面两点。堆内存和栈内存的区别在 CSAPP 里讲过，如果不了解的话可以询问 AI。

| **特性** | **基本数据类型** | **引用数据类型** |
| --------- | -------------------------------- | --------------------------------------- |
| **存储内容** | 直接存储**数值**本身。 | 存储数据的**内存地址**（引用），实际数据在**堆**中。 |
| **存储位置** | 通常存储在**栈** (Stack) 上（作为局部变量时）。 | 实际对象存储在**堆** (Heap) 上，栈中只存引用。 |
| **方法/属性** | **没有**方法（如 `.toString()`），也没有属性。 | 拥有方法和字段 (Field)，继承自 `java.lang.Object`。 |
| **默认值** | `0`, `false`, `\u0000` 等（取决于类型）。 | `null`。 |
| **内存开销** | 非常小，非常高效。 | 较大（包含对象头、元数据等开销）。 |

我们用这个例子来帮助我们理解基本数据类型和引用数据类型最大的差别。

```java
int a = 10;
int b = a;    // 值复制，b 和 a 的值是相互独立的

int[] x = {1, 2, 3};
int[] y = x;  // 引用复制，x 和 y 指向*同一个*数组对象，就好比 C++ 里两个指针指向同一个地址一样
y[0] = 99;
System.out.println(x[0]); // 输出应该是 99，因为修改 y[0] 表示的是修改“y指向的数组对象”在下标 0 处的值！
```

### 变量、标识符、关键字

这块应该是送分题，注意别看错就行。

#### 标识符规则

跟 C++ 差不多，标识符不能随便取。

* 字母、`_`、`$` 开头都行；
* **不能**以数字开头；
* **严格**区分大小写：`myName` 和 `MyName` 不是一个东西；
* **可以**使用 Unicode 字符，比如 **π**，中文，甚至🍺，但**不建议**用于实践。

Java 程序员广泛接受的命名规范大致有下面两种：

* 类名：`PascalCase`（所有单词首字母大写）`StudentManager`；
* 变量/方法：`camelCase`（第一个单词首字母小写，后续首字母大写）`numberOfStudents`。

### 字面值

#### 整数

* 十进制：`123`；
* 二进制：`0b1010`；
* 八进制：`012`（所以前导 0 很危险，可能会让你的数字表示**不同**的含义！）；
* 十六进制：`0xFF`。

`long` 的字面值如果超出 `int` 范围，末尾必须加 `L`：

```java
long big = 12345678900L;
```

#### 浮点数

* 默认**都是** `double` 类型；
* `float` 要加 `f/F`

```java
double d = 3.14;
float f = 3.14f;
```

#### 字符

* 类型为 `char`；
* 用单引号包裹，比如：`'A'`；
* 注意其本质是 Unicode 编码，也就是说`char` 其实就是个 **16 位无符号整数** (unsigned short)。

```java
char c = 'A';
System.out.println((int)c); // 65
```

16 位无符号整数已经足够编码常见的字母和汉字，但是也存在表示不了的字符，比如一些生僻符号和表情符号：

```java
// char c = '🍺'; // ❌ 编译报错，Emoji 超出了 char 的单字符范围
String s = "🍺"; // ✅ 正确，这是由两个 char 组成的
System.out.println(s.length()); // 输出 2
```

#### 布尔

就俩值：`true` / `false`。需要注意的是 Java 不像 C++ 一样允许你在需要一个布尔值的地方放一个整数（其实是不会隐式地把整数转换为bool），比如：

```java
int a = 100;
while (a) { // 非法，不许隐式转换，
    // Do something...
   a--;
}
```

#### null

`null` 只能给引用类型，也是引用类型的默认值：

```java
String s = null;
// int x = null; // 编译错误
```

### String

这一章你暂时只用知道下面 3 点，后面会有[单独的一章](##字符串)讲字符串。

* 字符串字面值用双引号：`"Hello"`
* `+` 可以拼字符串（会把别的类型先转成字符串）
* 字符串字面值不能直接换行（长的用 `+` 拼，或去第 6 章学 Text Block）

```java
int age = 20;
System.out.println("age = " + age); // "age = 20"
```

### 数据类型转换

在对不同数据类型的值做操作时需要进行数据类型转换，有两种情况：

* 从表示范围大的转换到表示范围小的；

* 从表示范围小的转换到表示范围大的。

后者有些时候可以自动进行（隐式类型转换），前者必须由你**手动写明**强制转换。

这个转换问题我们在[继承那一块](##继承)也会讨论很多。

#### 自动类型转换

位数少的 → 位数多的，不用你来手写，编译器可以自动帮你做：

```java
byte b = 120;
int i = b;     // OK
double x = i;  // OK
```

#### 强制类型转换

位数多的 → 位数少的，可能**丢数据/溢出**，所以必须由你来指定要这么转换，你要**承担**转换溢出的风险。

```java
double d = 200.5;
byte bb = (byte)d;
System.out.println(bb); // 很可能是负数（溢出截断）
```

强转缩窄**只保留低位**，超出的高位直接砍掉，砍完按补码就可能变成负的了。

#### *表达式类型提升

注意这里是个坑！`byte/short/char` 做算术运算时，会**先提升**成 `int`，如果考试的时候你写出了第三行的代码可能是会被判错的！

```java
byte a = 40;
byte b = 50;
// byte c = a + b; // 编译错误！a+b 是 int！
byte c = (byte)(a + b); // OK（但你要自己承担溢出风险）
```

### 运算符

这里基本都在C++见过了，不多赘述，自己看一下就行。

**优先级不必死背，必要时加括号**。

#### 关系运算符

各种比大小，判断相等/不等，`> < >= <= == !=`。其中 `==` 的细节我们在后面会提到。

结果是 `boolean`。

#### 逻辑运算符

注意 `& |` 可以是逻辑运算符也可以是位运算符。和我们经典的逻辑运算符相比区别是

* `&&` / `||` 会**短路求值**，左边能决定结果就不算右边；
* `&` / `|`：不短路，两边都算。

短路可能会考这种：

```java
String s = null;
if (s != null && s.length() > 0) {
    // OK：s 为 null 时，右边不算
    // 没有短路的话这里会出现空引用异常
}
```

#### 位运算符

`& | ^ ~ << >> >>>`

考试一般不会让你手搓复杂位运算，但可能会问你“这玩意是干啥的”。另外也有一些用移位运算代替关于 2 的次幂乘除法的小伎俩，上过 csapp 的同学应该很熟悉，没上过的可以看下面的例子：

```java
int a = 5;
// 左移 1 位 = 乘以 2^1 = 乘以 2
System.out.println("5 << 1  = " + (a << 1)); // 结果 10
// 左移 3 位 = 乘以 2^3 = 乘以 8
System.out.println("5 << 3  = " + (a << 3)); // 结果 40


int b = 16;
// 右移 1 位 = 除以 2^1 = 除以 2
System.out.println("16 >> 1 = " + (b >> 1)); // 结果 8
// 右移 2 位 = 除以 2^2 = 除以 4
System.out.println("16 >> 2 = " + (b >> 2)); // 结果 4

// 【注意】负数的行为
int c = -16;
System.out.println("-16 >> 1 = " + (c >> 1)); // 结果 -8 (保留了负号)

// 对于正数，>>> 和 >> 效果一样
int d = 16;
System.out.println("16 >>> 1 = " + (d >>> 1)); // 结果 8

// 【大坑】对于负数，>>> 会把符号位也一起移动，并在高位补 0
// 这会导致负数瞬间变成一个巨大的正整数！
int e = -16;
System.out.println("-16 >>> 1 = " + (e >>> 1)); 
// 结果: 2147483640 (不再是除法了)
```

#### 三元运算符

也叫条件运算符，它可以把 if-else 写成表达式：

```java
int max = (a > b) ? a : b;
```

### 注释与代码风格

跟 C++ 差不太多，可能就多了个文档注释。

```java
// 单行注释

/* 多行注释 */

/**
 * 文档注释（javadoc，可以用工具导出成html格式的api文档）
 */
```

代码风格上最基本的一些共识：缩进统一（常见 4 空格），二元运算符两边留空格等。但是我们都手写代码了，所以只要你别全写到一行，或者用反人类缩进，就应该问题不大。

---

## 结构化编程

基本上跟 C++ 一模一样，简单过一下即可。

### 选择结构 if

#### 单分支 if

```java
if (a > b) {
    int t = a;
    a = b;
    b = t;
}
```

Tip：哪怕只有一行，**也建议**加大括号，不然在笔试的时候你可能被自己的缩进**（或者题目的缩进）**坑到！

#### 双分支 if-else

略。

#### 多分支 else-if

略。

### 条件运算符

上一节讲过了，跟 C++ 一样的。

```java
String msg = (score >= 60) ? "pass" : "fail";
```

### switch

最经典的 switch 和 C++ 是一样的，而且会**有直通**行为。

```java
int a = 0;
switch (a) {
    case 0:
        System.out.println("a is zero");
        break;
        // 如果没有这个break，那么当a为0时匹配到case 0后会继续往下执行case 1
    case 1:
        System.out.println("a is one");
        break;
    default:
        System.out.println("a is neither zero nor one");
}
```

PPT 上讲的是这种更现代的 switch，使用 `->` 连接标签和语句（块），注意这里的语句块就是用大括号括起来的多条语句：

```java
Scanner input = new Scanner(System.in);
System.out.print("输入一个季节（1,2,3,4）：");
int season = input.nextInt();  
switch (season) {
   case 1 -> System.out.println("春雨惊春清谷天");
   case 2 -> System.out.println("夏满忙夏暑相连");
   case 3 -> System.out.println("秋处露秋寒霜降");
   case 4 -> System.out.println("冬雪雪冬小大寒");          
   default -> System.out.println("季节输入非法.");
}
```

此外，PPT 上还提到了 switch 表达式，注意表达式是**可以返回值**的：

```java
DayOfWeek day = DayOfWeek.SATURDAY; // 这里是个枚举，我们后面会讲
int numLetters = switch(day){
    case MONDAY, FRIDAY, SUNDAY -> 6;
    case TUESDAY                -> 7;
    case THURSDAY, SATURDAY     -> 8;
    case WEDNESDAY              -> 9;
};
System.out.println(numLetters);        // 输出：8
```

下面的例子演示了比较复杂的 switch 表达式用法。

```java
Scanner input = new Scanner(System.in);
System.out.print("输入一个年份：");
int year = input.nextInt();
System.out.print("输入一个月份：");
int month = input.nextInt();
int numDays = switch (month) {
    case 1, 3, 5, 7, 8, 10, 12 -> 31;
    case 4, 6, 9, 11 -> 30;
    // 对2月需要判断是否是闰年
    case 2 -> {
    if (((year % 4 == 0) && !(year % 100 == 0)) || (year % 400 == 0))
        yield 29; // yield 表示为这个“表达式”返回“值”
            // 不能写 return，因为这个 switch 块不是函数。
    else
        yield 28;
    }
    default -> 0;
}; 
System.out.println("该月的天数为：" + numDays);
```

### 循环

#### while

跟 C++ 没区别，不懂的问 AI。

#### do-while

同上

#### for

也同上，注意可以有多变量的 for，其实也和 C++ 一样：

```java
for (int i = 0, j = 10; i < j; i++, j--) {
    // ...
}
```

#### 增强 for

长得很像 C++11 的 range-based for，而且更 Java，比如这里的 x 是**值拷贝**，不能用于修改值。另外，使用增强 for 访问一个[集合类](###集合)的期间不能修改集合，否则会产生运行时异常。

```java
int[] arr = {1, 2, 3};
for (int x : arr) {
    System.out.print(x + " ");
}
// 输出：1 2 3

int[] arr = {1, 2, 3};
for (int x : arr) {
    x = 100; // 只修改了局部变量 x
}
System.out.println(Arrays.toString(arr));
// 输出：[1, 2, 3]

List<Integer> list = new ArrayList<>(List.of(1, 2, 3));
for (int x : list) {
    if (x == 2) {
        list.remove(x);   // 运行期异常
    }
}
// 异常：Exception in thread "main" java.util.ConcurrentModificationException
```

### break / continue

也跟 C++ 一样。break 还能跳出 switch 块。break 有一个 break label 的语法可以选择要跳出哪层循环，类似 goto，但是笔者非常不建议你自己这么写。

```java
// PPT 上的 example
start:
for(int i = 0; i < 3; i++){
    for(int j = 0; j <4; j++){
       if(j == 2){
          break start;      // 跳出start标签标识的循环
      }
       System.out.println(i +":" + j);
    }
}

```

---

## 类、对象和方法

这章是**面向对象**的基础，后面的**继承、多态、接口**全部以这一章为基础。如果你在学 C++ 的时候就学得不是很清楚，笔者建议你多花点时间消化一下。

Java 的类与对象部分会比 C++ 更**简单**，但是需要注意语法、习惯上的差别。

### 面向对象到底在干什么

类是对象的**蓝图**（或者说，抽象），对象是类的**实例**。

* 类 (class)：描述对象“应该有什么字段/方法”
* 对象 (object) ：运行时真正存在的一坨数据 + 行为

如果你还是不太弄得懂的话我用下段话再解释解释，已经明白的可以跳过去。

> 如何去写一个类其实取决于你**想要**如何去研究一个对象。比如说，对于”人”，在学校里我们可能希望研究 ta 作为学生的属性，比如学号、年级、姓名、性别，以及一些行为，比如读书、写作业，那么你的定义可能是下面这样的：
>
> ```java
> class Student {
>       private String name;
>       private Gender gender;
>       private long id;
>       private int grade;
>       public void Read() {}
>       public void DoExercises() {}
> }
> ```
>
> 其实在你这样做的时候，你已经不自觉地**隐去**了所有无关的细节。比如：
>
> * 这个人的父母是谁？
> * 这个人出生在哪里？
> * 这个人家住在哪里？
> * 这个学生有什么兴趣爱好？
> * 这个学生和别的学生有什么社交关系？
> * ...
>
> 如果你还想研究别的问题（比如家庭住址），你可能就会再添加一个字段表示 ta 的住址，或者添加一个方法表示 ta 做某件事。而在公安局之类的场景里，以上的信息可能都在他们的研究范围内，所以他们抽象出来的 Student 会更加复杂。
>
> 看到了吗？这就是对具体研究对象的**抽象**。你不关注无关的信息，仅提取出你关注的、这一类对象所**共有**的特征（数据和行为），定义成了一个**类**。
>
> 而类可以作为“蓝图”去构造新的对象，比如：
>
> ```java
> Student stu = new Student("张三", 19);
> ```
>
> 就可能表示一个名叫“张三”的学生，年龄19岁。这时 new 产生的就是 Student 类的实例，也就是**对象**。

### 定义类与创建对象

一个基本的类的定义大概长这样，上面那些变量声明是**字段 (field)**，下面那些语句块的部分是**方法 (Method)**。

```java
public class Employee {
    private String name; // 字段，表示对象的数据
    private int age;

    public void sayHello() { // 方法，表示对象的行为
        System.out.println("My name is " + name);
    }
}
```

字段、方法等可以统称为类的**成员**（别的成员我们后面会提到，主要是这两种）。类的成员都可以被**访问修饰符**修饰，比如上面的 `private` 和 `public`。另外，普通的方法和字段还可以声明为**静态 (static)**，我们后面都会讲到。

创建对象的语句如下，其实 `new` 操作符调用了它的构造方法，我们很快就会讲到。

```java
Employee e = new Employee(); // new 才会在堆里构造对象
```

不过这里这个对象除了可以调用 `e.sayHello()` 之外没有别的用途了，无法设置它的 `name` 和 `age` 字段。我们后面会介绍更多内容来让它变得更“有用”。

### 栈与堆

C++ 里对象可能在栈上也可能在堆上，但是 Java 的情况比较简单：

* 局部变量（比如 `e`）在**栈**；
* `new Employee()` 出来的对象本身（和对象里的数据）在**堆**；
* `e` 里保存的是“**指向堆对象的引用**”。

这也是为什么“引用赋值”会共享同一个对象。参考我们[之前](###基本类型与引用类型)在讲数据类型的时候的例子：

```java
int[] x = {1, 2, 3};
int[] y = x;  
 // 引用复制，x 和 y 指向*同一个*数组对象，就好比 C++ 里两个指针指向同一个地址一样
y[0] = 99;
System.out.println(x[0]); 
 // 输出应该是 99，因为修改 y[0] 表示的是修改“y指向的数组对象”在下标 0 处的值！
```

### 方法

#### 定义方法

在前面，我们提到对象除了数据属性之外还具有**行为**属性，通常我们使用方法来描述对象的行为。我们马上要提到的构造方法就可以看作一种特殊的方法。这一节中，我们先介绍一般的方法：

```java
[访问修饰符] [static] 返回类型 方法名(参数列表) {
    // 方法体
}
```

返回类型可以是我们之前提到的基本数据类型或引用数据类型，或者是 void，表示不返回任何东西。比如：

```java
public class Employee {
    private String name;
    private int age;

    public void sayHello() { // 方法，表示对象的行为
        System.out.println("My name is " + name);
    }
}
```

然后你就可以在 `Employee` 对象上调用这个方法：

```java
Employee employee = new Employee();
employee.sayHello();
```

参数列表是这个方法接受的参数。定义变量时，这些“有名字”的变量我们称为**形式参数**，比如：

```java
public class Employee {
    private String name;
    private int age;

    public int calcSalary(int base, int bonus) {
       return base + bonus; // 这里的 base 和 bonus 是形参（形式参数）
    }
}
```

然后我们就可以这样调用它并接受它的返回值。调用方法时传入的**实际的值**称为**实际参数**。

```java
int salary = employee.calcSalary(8000, 3000); // 这里的 8000 和 3000 是实参（实际参数）
```

#### 属性

下面是一种在 Java 中**约定俗成**的写法（但不是语法！），称为**属性**，将字段设为 private，然后通过对应的一组 Getter/Setter 方法来访问它：

```java
public class Employee {
    private String name; // 字段，表示对象的数据

    public String getName() { // getter
        return this.name;
    }
  
   public void setName(String name) { // setter
        this.name = name;
    }
}
```

#### *方法重载

你可以定义名称相同、但参数列表不同的多个方法来处理各种不同的输入参数。“参数列表不同”指参数的个数、类型、顺序至少有一个不同。但是注意，**参数名称不同/返回值不同不构成重载！**

> 参数列表中的“参数名称”本就不是“方法签名”的一部分。编译器看是否为重载看的是方法签名。

```java
void f(int x) {}              // 合法
void f(int y) {}              // 非法：参数名不同不构成重载

void f(double x) {}           // 合法，参数类型不同
void f(int x, int y) {}       // 合法，参数个数不同

char f(int x) {}              // 非法：返回值不同不构成重载
char f(char x) {}             // 合法，参数类型不同

void f(int x, double y) {}    // 合法
void f(double x, int y) {}    // 合法，参数顺序不同
```

#### *方法的参数传递

方法参数传递在哪个语言里都是不得不提的话题。总体来讲 Java 的方法参数**总是采用值传递**。也就是说参数是传入值的复制，而不是“本身”。我们用两个例子来说明这一点。

如果传入的是基本数据类型，那很好理解，`num` 的值是 `x` 的**复制**，在 `change()` 方法中修改参数 `num` 不影响原本调用它时传入的 `x`。

```java
static void change(int num) {
    num = num * 2;
}

public static void main(String[] args) {
    int x = 100;
    change(x);
    System.out.println(x); // 100
}
```

不过，如果传入的是引用类型，那就必须要注意了，传入的是**引用的复制**，你可以理解成 C++ 的**复制了一个指针**，堆上的对象只有一个，但是复制出来了一个新的指针来指向对象。

```java
static void change(Employee emp) {
    emp.setName("Alice");     // 改对象内容：外面看得到
    emp = new Employee();     // 改引用指向：外面看不到
}

public static void main(String[] args) {
    Employee e = new Employee();
    change(e);
    // e 的 name 变为Alice，但 e 仍然指向原对象
}
```

下面是请 AI 画的图，你再看不懂那我也没辙了。

```plain
栈（Stack）                    堆（Heap）
+-------------------+          +----------------------+
| main              |          | Employee 对象 A      |
| e ────────────────┼─────────▶| name = null          |
+-------------------+          +----------------------+
// 这是调用 change 前

栈（Stack）                    堆（Heap）
+-------------------+          +----------------------+
| change            |          | Employee 对象 A      |
| emp ──────────────┼─────────▶| name = null          |
+-------------------+          +----------------------+
| main              |
| e ────────────────┼─────────▶ 还是上面那个对象A
+-------------------+
// 这是调用 change 时
```

#### 递归

Java 的方法也是可以递归调用的，不过应该不是考试的重点，这里只是顺带一提。

### 构造方法

每个类都有**构造方法 (constructor)**，构造方法用来创建类的对象或实例。构造方法也有名称、参数和方法体，你可以把它理解为一种特殊的方法。

* 构造方法主要作用是创建对象并初始化类的成员变量。

* 对类的成员变量，**若声明时和在构造方法都没有初始化，新建对象的成员变量值都被赋予默认值。**

* 对于不同类型的成员变量，其默认值不同。整型数据的默认值是 `0`、浮点型数据默认值是 `0.0`、字符型数据默认值是 `’\u0000’`、布尔型数据默认值是 `false`。

* 引用类型数据默认值是 `null`。

#### 定义构造函数

构造方法必须是满足以下规则的方法：

1. 名字必须和类名完全一样；
2. 没有返回值，连 `void` 也**不能**写！
3. 用 `new` 调用。

```java
public class Employee {
    private String name;
    private int age;

    public Employee() {          // 无参构造
        this.name = "";
        this.age = 0;
    }

    public Employee(String name, int age) { // 有参构造
        this.name = name;
        this.age = age;
    }
}
```

#### 默认构造方法

所有类在默认情况下都是有一个无参默认构造方法的，也就是说代码：

```java
public class Employee {
    private String name;
    private int age;
}
```

和下面的代码等价：

```java
public class Employee {
    private String name;
    private int age;
   public Employee() {} // 什么都不做的默认构造函数
}
```

但是，一旦你自己写了构造函数，编译器就**不会**再为你生成这个默认构造函数。

例如，在下面的代码里，调用无参构造函数会报错：

```java
public class Employee {
    private String name;
    private int age;
    public Employee(String n, int a, double s) {
     name = n ;
      age = a ;
      salary = s ;
    }
}

// 在另一个文件的 main 函数里
// Employee emp = new Employee(); // 报错
```

这种情况下，你可以利用[之前](####*方法重载)我们提到的**方法重载**再写一个无参的构造函数。

```java
public class Employee {
    private String name;
    private int age;
    public Employee(String n, int a, double s) {
     name = n ;
      age = a ;
      salary = s ;
    }
    public Employee(){
      name = "";
      age = 0;
      salary = 0.0;
    }
}
```

#### *this 关键字

讲到了构造函数，我们不得不提 this 关键字。this 关键字有两种基本的用途：

1. 区分成员变量和同名局部变量/参数；
2. 在构造方法里调用另一个构造方法：`this(...)`。

```java
// 1. 左侧 this.name 表示成员变量，右侧是参数
public Employee(String name, int age, double salary) {
    this.name = name;
    this.age = age;
    this.salary = salary;
}

// 2. 在构造方法里调用另一个构造方法
public Employee(String name) {
    this(name, 0); // 调用另一个构造
}
```

需要注意，只有在构造方法和实例方法里才有 this，因为 this 其实表示的是**对当前对象的引用**，而在静态方法中是没有“当前对象”的。静态方法我们马上就会提到。

### UML类图

懒得喷，不做，自己看 PPT 😠。

### 静态成员

我们前面提到类的成员可以用 `static` 修饰，这样的成员称为**静态成员**。它们是**依附在类本身上**的，与前面我们提到的**依附于对象**（也就是类的实例）的成员相对。

#### 静态字段

比如说我们可以这样使用一个静态字段，用来统计 `Counter` 对象的个数：

```java
public class Counter {
    public static int count = 0; // 不依赖于任何 Counter 对象，独立存在
    public Counter() { count++; } // 每次构造一个 Counter 对象时，让这个静态的计数器自增
}

Counter counter1 = new Counter(); // count = 1
Counter counter2 = new Counter(); // count = 2
System.out.println(Counter.count); 
 // 输出：2
 // 访问静态成员时使用类名作为前缀

// PS：Java 允许你通过实例访问静态成员，但一般不建议你这么做
System.out.println(counter1.count);  // 可以编译和正常运行
```

#### 静态方法

静态方法常用来写静态工厂方法或一些工具方法，下面是一个 Java 核心类库中 LocalDate 的例子：

```java
LocalDate today = LocalDate.now();
// LocalData 的静态方法 now() 可以返回一个表示当前日期的 LocalDate 对象
```

再比如 `Math` 类中的各种静态工具方法，它们**不需要**依附于 `Math` 的实例的其他成员，所以定义为静态的：

```java
double x = Math.sqrt(16);      // 4.0
double y = Math.pow(2, 3);     // 8.0
double z = Math.max(10, 20);   // 20.0
```

你可以对比一下，下面这种写法和上面的谁看着更自然：

```java
Math math = new Math(); // 创建对象，但是你也不知道有什么东西需要初始化
            // 所以 Java 的 Math 其实通过私有化默认构造函数：
            // private Math() {}
            // 阻止了你实例化 Math 类，也就是说语法上你也不能这么写
double x = math.sqrt(16);
double y = math.pow(2, 3);
double z = math.max(10, 20);
```

#### 静态成员的访问限制

在不考虑访问性修饰符影响的情况下：

* 实例方法可以调用其他实例方法/字段和静态方法/字段
* 静态方法可以调用其他静态方法/字段，但**不能调用其他实例方法/字段**。

其实很好理解，因为静态方法本身**不依附于实例**，它如果访问类别的实例方法/字段，那这个“实例”从哪来呢？所以静态方法不能调用其他实例方法和字段。

```java
class Demo {
    int x = 10;              // 实例字段
    static int y = 20;       // 静态字段
    // 实例方法
    void instanceMethod() {
        // 访问实例成员
        System.out.println(x);        // OK
        instanceHelper();             // OK
        // 访问静态成员
        System.out.println(y);        // OK
        staticHelper();               // OK
    }
    void instanceHelper() {
        System.out.println("instanceHelper");
    }
    // 静态方法
    static void staticMethod() {
        // 访问静态成员
        System.out.println(y);        // OK
        staticHelper();               // OK

        // 访问实例成员（不允许）
        // System.out.println(x);      // 编译错误
        // instanceHelper();           // 编译错误
    }
    static void staticHelper() {
        System.out.println("staticHelper");
    }
}
```

#### 常量

Java 的 `const` 是一个**保留字**，但和 C++ 不同，**不具有实际含义**。所以我们一般用 `final` 关键字来定义常量。下面这是一个典型的定义类级别常量的写法：

```java
public static final double PI = 3.1415926; // 类级别的常量
```

既然提到了 `final` 我们顺带介绍一下它的含义：

1. 修饰变量：值不能再变；

2. 修饰引用变量：**引用不能变，但对象内容可以变**；

3. 修饰方法：不能被重写；

4. 修饰类：不能被继承。

后面两种我们在[继承](###继承)那一块还会深入讲解。

### 对象初始化的顺序

#### 实例成员的初始化顺序

PPT 上提到实例变量的初始化有多种方式：

* 声明时初始化；
* 使用**初始化块；**
* 使用构造方法初始化；
* 未被上述方法初始化时，自动初始化为**默认值**。

其中，初始化块的语法如下所示：

```java
class InitDemo {
   // 声明时初始化，这里的print可以调用，因为它是静态的
    int i1 = print("instance variable i1");

   // 初始化块，就是在类中由大括号包裹的一组语句块
    {
        print("instance block");
    }

    int i2 = print("instance variable i2");

    // 构造方法
    public InitDemo() {
        print("constructor");
    }

    private static int print(String msg) {
        System.out.println(msg);
        return 0;
    }
}

// ...
public class Test {
    public static void main(String[] args) {
        new InitDemo();
    }
}
```

在 `main` 中使用 `new` 关键字构造 `InitDemo` 的对象， 触发初始化，输出顺序如下：

```plain
instance variable i1
instance block
instance variable i2
constructor
```

简言之，声明时初始化和初始化块**严格按照它们在类中出现的顺序执行**，构造方法**始终最后执行**。

#### 静态成员的初始化顺序

静态初始化块就是在大括号前面加个 `static` 关键字：

```java
class StaticInitDemo {
    static int s1 = print("static variable s1");

   // 静态初始化块
    static {
        print("static block");
    }

    static int s2 = print("static variable s2");

    private static int print(String msg) {
        System.out.println(msg);
        return 0;
    }
}

// ...
public class Test {
    public static void main(String[] args) {
        System.out.println("main start");
        new StaticInitDemo();   // 触发类加载
        new StaticInitDemo();   // 不再触发静态初始化
    }
}
```

不过和实例成员有个区别，静态成员的初始化发生于**类加载**时。通俗而言你可以理解为**“类首次被使用时”**，而且只初始化一次。所以，上面的 Demo 输出如下，只触发了一次静态初始化：

```plain
main start
static variable s1
static block
static variable s2
```

#### 混在一起了怎么办？

虽然这是个很无聊的考点，但是我们还是放一个例子在这里：

```java
class MixedInitDemo {
    // ===== 静态部分 =====
    static int s1 = print("static variable s1");
    static {
        print("static block");
    }
    static int s2 = print("static variable s2");

    // ===== 实例部分 =====
    int i1 = print("instance variable i1");
    {
        print("instance block");
    }
    int i2 = print("instance variable i2");

    // ===== 构造方法 =====
    public MixedInitDemo() {
        print("constructor");
    }

    private static int print(String msg) {
        System.out.println(msg);
        return 0;
    }
}

// ...
public class Test {
    public static void main(String[] args) {
        System.out.println("main start");
        new MixedInitDemo();
        System.out.println("---- second object ----");
        new MixedInitDemo();
    }
}
```

Java 遵循**先类后对象**，**先静态后实例**，**同一类别内按代码书写顺序执行**，**构造方法始终最后执行**的规则来执行初始化。所以我们可以看到，第一次构造对象时，先进行了静态初始化，然后是实例初始化；第二次调用时只进行实例初始化。

```plain
main start
static variable s1
static block
static variable s2
instance variable i1
instance block
instance variable i2
constructor
---- second object ----
instance variable i1
instance block
instance variable i2
constructor
```

这一堆令人头大的初始化顺序在后面聊到继承的时候还会出现。如果你弄不清楚就算了，考了大不了认栽。

### 作用域与生命周期

Java 的作用域与生命周期比起 C++ 应该更好理解一些。其中变量的作用域和 C++ 区别不大。

#### 作用域

一般而言，一个变量的作用域 (Scope) 就是它所在的一组大括号（也就是语句块）内。若一个变量属于某个作用域，它在该作用域可见，即可被访问，否则不能被访问。

直接搬一个 PPT 的示例吧：

```java
public class ScopeDemo {
      int i = 100;
      int[] a = { 1, 2, 3, 4 };
      public void method(int n) {
        int i = 3; // i 的作用域是整个 method 的方法体
        for (int j = 0; j < 3; j++) {
            a[j] = 0;
        } // j 的作用域在这里结束
        while (i > 0) {
             int tmp = i * i;
             a[i] = a[i] + tmp;
             i--;
        }
        Employee c = new Employee();
        System.out.println(i); // 局部变量 i 依然存在
        System.out.println(this.i); // 这是类的字段 i
        System.out.println(n + 8);
    } // 局部变量 i 和参数 n 的作用域在这里结束

    public static void main(String[] args) {
       new ScopeDemo().method(10);
       // 输出：
       // 0
       // 100
       // 18
    }
}
```

#### 生命周期

PPT 如下：

> 变量的**生命周期 (lifetime)** 是指变量被分配内存的时间期限。当声明一个方法局部变量时，系统将为该变量分配内存，只要方法没有返回，该变量将一直保存在内存中。一旦方法返回，该变量将从内存栈中清除，它将不能再被访问。对于对象，当使用new创建对象时，系统将在堆中分配内存。当一个对象不再被引用时，对象和内存将被回收。但**不是对象离开作用域就立即被回收**。实际上是在之后某个时刻当垃圾回收器运行时才被回收。

省流：

* 基本数据类型变量通常存放在栈中，其生命周期**与作用域一致**；
* 对象存放在堆中，离开作用域时不会立即销毁，而是在没有任何引用指向后在未来某一时刻被 GC 回收。

反正是比 C++ 手动管理内存要简单的。

### var 关键字

这个其实是个语法糖，有点像 C++ 的 `auto`（不知道就算了）。

它允许你在**声明局部变量**时让编译器根据右边的表达式值推测变量类型。注意，它**不是动态类型！**

```java
var x = 10;      // 推断为 int
var y = 3.14;    // 推断为 double
var flag = true; // 推断为 boolean
var s = "Hello";          // String
var list = new ArrayList<String>(); // ArrayList<String>
var today = LocalDate.now();  // LocalDate
var len = s.length();        // int
for (var i = 0; i < 3; i++) {
    System.out.println(i);
}
for (var x : list) {
    System.out.println(x);
}
```

但只能用于**局部变量声明且同时初始化**，不能先 `var x;` 再赋值：

```java
// var x;        // 编译错误，编译器无法在这一行推断出 x 的类型
x = 10;
```

也不能用于类的成员和参数：

```java
class Demo {
    // var x = 10;          // 编译错误，不能用于字段
    // void f(var a) { }    // 编译错误，不能用于参数
}
```

### GC 垃圾回收

前面已经提到，引用类型的对象在没有引用指向它后，未来某一时刻会被 GC 回收。有一个 `System.gc()` 方法可以立即**建议** GC 进行垃圾回收，但**不会保证立即开始**垃圾回收。

这块内容应该考得不多，有需要的同学可以自己看第四章的 PPT。

### 写在章尾

其实 Java 类的基本性质和 C++ 相似度非常高。有一个核心的不同是，Java 在结构上是**完全面向对象**的。它的所有方法必定在类中，所有可执行语句必定在方法中，所有变量要么是类的成员变量要么是方法的局部变量，不像 C++ 一样可以在文件里直接写一个函数，所以其实 Java 里需要弄清楚的类的东西比 C++ 要少很多，因为你不需要考虑那些“类之外的函数”的情况。

---

## 面向对象特征

我们对章节顺序略做调整，先把面向对象的“正餐”部分说完：封装、继承、多态，再带一下包和模块。

笔者在本章节中采用**介绍一个特性，就给出一个示例**的顺序，而不是 PPT 上先文字概述再给代码实现的顺序，因为这样更适合你进行连续阅读。

### 包与模块

后面的访问修饰符有很多跟包相关的概念，所以我们先介绍一下 Java 的包和模块。

如果你之前写 Java 的 `class` 就一股脑分一堆文件放在一个文件夹里，那恭喜你——你已经使用过默认包了。默认包的行为我们马上就会提到。

先明确几个概念：

* **包 (Package)**：给类分组、避免命名冲突，顺便还能做一层访问控制，马上就会提到；  
* **类库 (Library)**：别人（或你自己）写好的类的“打包”，一般以 `.jar` 的形式出现；  
* **模块 (Module)**：Java 9 之后的新封装维度，把一堆包再打成一个更大的包，还能声明依赖和导出范围。

#### 包

包名本质上就是一串逻辑意义上的“目录名”，通常建议用“域名反写”来保证不跟别人撞车，比如你有域名 `xy.boda.com`，那你的包名就可以叫 `com.boda.xy`（当然你没有域名也没关系，反正考试不管这个）。

Java 的包有点像 C++ 的命名空间，但是它具有**控制访问性**的功能。

源文件最上面的 `package` 语句就表示把下面的类放在这个包里。注意：

* 它必须是第一条**非注释**语句；
* 一个文件**只能写一条**。

比如说下面这个例子就把 `MathUtils` 这个类放到了 `com.example.util` 包中：

```java
package com.example.util;

public class MathUtils {
    public static int abs(int x) {
        return x >= 0 ? x : -x;
    }
}
```

不写 package 语句时，你的类就在**默认包**中，有两个特点：

* 默认包中的类“看起来很自由”，但其实很局限；

* 默认包的类很难被别的包正常 import；

看起来“自由”的点在于，在**同一个目录**下，默认包里的类可以互相访问，写作业的时候用默认的包可以方便地把几个 public 类分到不同的文件里以避免被编译器卡住（见下一节）：

```java
// A.java（默认包）
public class A {
}

// B.java（默认包）
public class B {
    A a = new A();  // OK，可以直接使用
}
```

但是，使用默认包的类**不能被有包名的类 import**，比如：

```java
// 文件：com/example/Test.java
package com.example;

// import A;   // 报错：不能 import 默认包中的类

public class Test {}
```

所以工程上几乎见不到用默认包的类文件，当然平时交作业的时候呢想怎么用就怎么用吧。  

#### 包与文件的澄清

初学者可能下意识把 **“一个 `.java` 文件”** 和 **“一个包”** 画上等号，其实，包是人定义的逻辑概念，而文件是包的物理载体。**一个文件里只能有一个包**，这是 Java 的语法规定的，和前文所说的一样；但是**多个文件中的类可以同属一个包**，只要在它们的文件开头分配同一个包即可。

另外，**工程上**经常将包名、类名和**目录结构**、文件名相对应。

比如下面这个目录结构可以表示三个**公开**的 `XXXUtils` 类均在包 `com.example.util` 中。

```plain
com/
 └─ example/
    └─ util/
       ├─ MathUtils.java
       ├─ StringUtils.java
       └─ DateUtils.java
```

但需要区分的是，目录结构与包名的绑定是工程实践，而**一个文件中至多有一个公开类，且公开类名称必须与文件名相同**是 Java **语言层面**的约束。

```java
// C.java
package com.example.util;

class A { }
class B { }
public class C { } // OK，公开类名称和文件名一致

// B.java
package com.example.util;

class A { }
class B { }
// public class C { } // 报错，公开类名称和文件名不一致

// A.java
package com.example.util;

class A { }
class B { } // OK，没有公开类
```

上面的例子演示了这一语法规则。

#### import 关键字

要想跨包使用类，你要么写包括包名前缀的全限定名：

```java
// 写全限定名（无论是考试还是写代码最好都别这么写）
java.util.Scanner in = new java.util.Scanner(System.in);
```

要么**在文件开头，package 语句后面**，使用 `import` 语句：

```java
// import（正常人写法，卷子上要用 Scanner 也要记得写 import）
// 在文件开头的 package 语句后面：
import java.util.Scanner;

// 在方法中：
Scanner in2 = new Scanner(System.in);
```

在 import 语句中可以使用通配符，比如 `import java.util.*;` 表示导入这个包里的所有类，但它的滥用可能导致下面这种经典的错误：

```java
import java.util.*; // 有 Date
import java.sql.*;  // 也有 Date

// Date d = new Date(); // 编译器报错：你到底想用哪个 Date？
```

还有一种花活叫静态导入（`import static`），可以让你直接用静态方法/常量，不用写类名前缀：

```java
import static java.lang.Math.*;

double x = sqrt(16);   // 不用写 Math.sqrt
double y = random();   // 不用写 Math.random
```

能看懂就行，这种静态导入会进一步增大命名冲突的风险，而且可读性比较差。

#### 模块

模块你只要记住它大概长这样就行：

* 模块描述符文件叫 `module-info.java`；  
* 常见关键词是 `requires`（我依赖谁）和 `exports`（我把哪些包开放出去）；  
* 不写模块的老项目会落在一个“未命名模块”里，照样能跑，属于 Java 的祖传兼容。  

你大概会看到这种结构：

```java
module com.example.app {
    requires java.base;          // 其实默认就有
    requires java.logging;       // 用到日志模块就写
    exports com.example.api;     // 只把 api 包开放给别人
}
```

模块系统要展开讲可以讲很多很多，有需要的可以看 PPT，这里就不展开了~~，反正笔者大胆推测期末不考这个。~~

### 封装与访问权限

面向对象三大特性之一的**封装性 (Encapsulation)** 讲人话其实是：把“怎么实现的”藏起来，只把“怎么用”露出来。一方面来说，用户不能随便修改你的类里不想让他们改动的字段；另一方面，用户无需关心你如何实现类的功能，只需要调用对应的方法即可，这提高了程序的模块化程度。

在 Java 里封装主要靠两件事：

* 用**包 (Package)** 把一堆类圈起来；  
* 用**访问修饰符**把类/成员的可见范围圈起来。

我们先来看访问修饰符。

#### 访问修饰符

我们前面已经大量地使用 `private`, `public` 这样的关键字修饰类的成员，它们就是**访问修饰符**，可以控制成员的可访问性。而在包的层面上，它们也可以控制类的可访问性。

#### *成员的访问权限

类的成员——字段/方法有四种访问级别，由小到大分别是：

* `private`：只有本类能访问；  
* **缺省**（不写修饰符）：同包能访问，也叫 package-private；  
* `protected`：**和C++不同！**同包就能访问，或不同包的**子类能访问**；
* `public`：哪里都能访问。  

一个非常常见的“封装写法”是：字段 `private`，通过 Getter/Setter 暴露出去（你前面已经见过了）。

这里要重点关注 `protected` 的含义，因为在 C++ 中它表示仅子类能访问，但在 Java 中：

* **同一个包**里，一个类的 `protected` 的成员可以被别的类的成员访问；  
* 不同包里，只有**继承关系**的子类能访问到 `protected`；

下面的例子演示了缺省和 `protected` 在不同文件的同一个包中的行为：

```java
// 文件：pkg1/A.java
package pkg1;

public class A {
    protected int p = 10;   // protected 成员
    int d = 20;             // 缺省（package-private）
}

// 另一个文件：pkg1/B.java
package pkg1;

public class B {
    void test() {
        A a = new A();
        System.out.println(a.p); // OK，同包，protected 可访问
        System.out.println(a.d); // OK，同包，缺省也可访问
    }
}

```

但如果出现了跨包，protected 是比缺省**更宽松**的，但是**需要注意访问父类成员的方式**！

```java
// 文件：pkg1/A.java
package pkg1;

public class A {
    protected int p = 10;   // protected 成员
    int d = 20;             // 缺省（package-private）
}

// 另一个文件：pkg2/B.java
package pkg2;

import pkg1.A; // 导入 pkg1 中的 A 类

public class B extends A { // B 是 A 的子类
    void test1() {
        A a = new A();
        // System.out.println(a.p); // 编译错误！B 是 A 的子类，但此处 this 和 a 无“关联”
        // System.out.println(a.d); // 编译错误！缺省不允许跨包访问
    }
   void test2() {
       System.out.println(this.p); // OK，这里的 p 在*没有被子类覆盖的情况下*是父类的
       System.out.println(p); // 不混淆，不加 this 也可以
       System.out.println(super.p);  // OK，最清晰的写法，显式访问父类的 p
    }
}

```

而 `private` 和 `public` 的行为比较简单，和 C++ 基本一致，所以此处不再赘述。

#### 类的访问权限

在包的层面上，访问修饰符可以控制类能否被别的包中的类访问。

一个直接放在 `.java` 文件中的类，也就是**顶层类**，**只有两种**选择：

* `public`：其他包也可以访问，而且**只能有唯一一个**；
* 缺省（什么都不写）：只有**同一个包**里的其他类可以访问这个类。  

你可能会问，这里为什么没有 `private` 呢？那让我们来看下面的例子：

```java
package pkg1;
class A {} // OK，只有 pkg1 里的其他类可以使用它
public class B {} // OK，其他包中的类可以导入并使用它
// private C {} // 编译错误，你想对谁 private？
```

很显然，在顶层类的场景里，`private` 和 `protected` 是没有意义的，**它们在语法上被禁止**。如果你需要控制一个类只能被另外一个特定类访问，可以使用我们后面会讲到的内部类来实现。

### 访问权限的层级嵌套问题

笔者特别地列出本章帮助你理解在一串访问链上的访问权限问题。

访问修饰符控制的是“**谁能直接看到某个声明**”，但在实际程序中，访问往往是**通过方法调用层层传递的**。因此，一个成员是否能被“间接使用”，并不只取决于它自身的访问级别，还取决于**对外暴露的 API 设计**。

我们直接来看例子。

#### 成员访问

##### 在 public 方法中返回 private 成员

可以，而且工程上经常这样做。在下面的例子中，外界可以通过 getter 方法获得 `name` 但无法直接访问字段本身以随意修改，这是典型的**只读**属性。`protected` 也同理

```java
package pkg1;

public class User {
    private String name;

    public User(String name) {
        this.name = name;
    }

    public String getName() {   // public 方法
        return name;            // 返回 private 字段的值
    }
}
```

##### *在 public 方法中返回可变引用类型的 private 成员

语法上**可以**，但事实上破坏了对 `data` 的封装性！这是一个很容易犯的错误！

外部代码：

* 不能直接修改 `b.data` 来改变所指向的数组对象；
* 可以通过 `getData()` 返回的**引用**来修改数组对象；
* 但依然不能直接改变 `b.data` 的引用！比如 `b.data = newData`。

```java
package pkg1;

public class Box {
    private int[] data = {1, 2, 3};

    public int[] getData() {
        return data;   // 注意：返回的是对数组对象的引用！
    }
}

// 在别的地方
Box b = new Box();
b.getData()[0] = 100; // 外部代码可以修改 Box 对象内部的私有字段 data
```

这里有一个额外的澄清，如果上面的两个例子引起了你的困惑，说明你对引用类型的理解已经相当到位。如果现在看不懂也没关系，后续涉及时我们还会深入探讨。

问题：`String` 明明也是引用类型，为什么第一个例子里不会出现第二个例子的问题？

答案：`String`, `Integer` 也是引用类型，但它们是**不可变对象**，外部代码**无法通过已有引用修改其内部状态**；而数组和大多数集合是**可变对象**。看下面的代码，在第一个例子返回 `String` 对象后，如果外部代码修改它：

```java
User user1 = new User("Alice");
String userName = user1.getName(); // 此时 userName 和 user1.name 指向同一个对象
userName = "Mr. " + userName; // 注意！试图修改时其实创建了新的 String 对象！
System.out.println(user1.getName()); // 不受影响
```

#### 跨包的类访问

这里主要探讨跨包传递缺省类实例的问题。

```java
// pkg1/Factory.java
package pkg1;

class Helper { }   // 缺省类

public class Factory {
    public Helper create() {
        return new Helper(); // OK
    }
}
```

在另一个包中使用 `Factory` 类的 `create()` 方法：

```java
// pkg2/Test.java
package pkg2;

import pkg1.Factory;

public class Test {
    void test() {
        Factory f = new Factory();
        // Helper h = f.create(); // 编译错误！Helper 不可见！
       Object h = f.create(); // OK，这个合法
    }
}
```

事实上，这里的编译错误是因为前面的 `Helper` 类对 `Test` 类是不可见的（换用 `var` 也一样）。不过依然可以用在这里可见的 `Object` 类或其他父类类型的引用承接返回的对象。

### 继承

终于到了这个老大难话题，也是面向对象的精髓部分。

**继承 (Inheritance)** 是“把公共的东西放到父类里，子类**自动拥有**”的机制。

语法上使用 `extends` 关键字，跟 C++ 的冒号语法不一样：

```java
class Person {
    protected String name;
    public void sayHi() {
        System.out.println("Hi, I'm " + name);
    }
}

class Employee extends Person {
    private int id;
}
```

注意：

* Java 只支持**单继承**（一个类最多 `extends` 一个父类）；  

```java
class A { }
class B { }

// 编译错误：不能同时继承多个类
// class C extends A, B { }

```

* 不写 `extends` 的类**默认**继承 `Object`；  

```java
class Person {}

// ...
Person p = new Person();
// 下面这些方法来自于 Object 类，后面会专门说
p.toString();
p.equals(p);
p.getClass();
```

* 子类继承父类中**非 private** 的成员（注意：**构造方法比较特殊，后面单独说**）。

这个例子演示了 protected 和 public 成员的行为：

```java
class Person {
    protected String name;
    public void sayHi() {
        System.out.println("Hi, I'm " + name);
    }
}

class Employee extends Person {
    void work() {
        System.out.println(name); // OK，继承来的 protected 字段
        sayHi();                  // OK，继承来的 public 方法
    }
}
```

父类的 private 成员子类是不能访问的：

```java
class Person {
    private int age;
}

class Employee extends Person {
    void test() {
        // System.out.println(age); // 编译错误：private 成员不可访问
    }
}

```

构造函数的特殊性，后面单独**重点**说：

```java
class Person {
    Person(String name) {
        this.name = name;
    }
}

class Employee extends Person {
    Employee(String name) {
        super(name); // 调用父类构造方法
    }
}

```

* 构造方法 **不会被继承**；

* 但子类构造方法**必须调用父类构造方法**。

`super(...)` 这一点后面会单独展开。

#### 方法重写

子类可以写一个“同名同参同返回类型”（返回类型也可以是协变的，后面再说）的方法，把父类的方法覆盖掉，这叫**方法重写 (Override)**。请务必和[之前](####*方法重载)的**方法重载 (Overload)** 区分开！

```java
class Person {
    public void work() {
        System.out.println("Person working...");
    }
}

class Programmer extends Person {
    @Override
    public void work() { // 覆盖父类 work()
        System.out.println("Programmer writing code...");
    }
}
```

重写时你需要遵守几条规则：

* 方法签名要**完全**一致（方法名 + 参数列表）；  

```java
class Person {
    public void work() { }
}

class Programmer extends Person {
    // @Override
    public void work(String task) { } // 这不是重写，是重载
}
```

`@Override` 其实是一种防呆注解，避免你误认为自己在重写而实际是重载，这种情况下编译器会把你拦住。

* 访问权限不能变得更小（父类是 public，你不能偷偷改成 protected）；  

```java
class Person {
    public void work() { }
}

class Programmer extends Person {
    // @Override
    // protected void work() { } // 编译错误：访问性变小
}
```

* 抛出的检查异常不能比父类更“宽”（后面异常处理那一章再详细说）。

```java  
import java.io.IOException;

class Person {
    public void work() throws IOException { }
}

class Programmer extends Person {
    @Override
    public void work() throws IOException { } // OK
    // public void work() throws Exception { } // 编译错误，检查异常变宽了
}
```

要想搞清楚方法重写的行为我们得先理解**多态**性，所以这个我们稍微往后推一推，先看看万恶的继承构造问题。

#### *super 关键字

`super` 是“父类那份**东西**”的入口，注意**不是“父类对象”**，而是“**当前对象里继承来的父类部分**”。

* `super.xxx`：访问父类成员；  
* `super(...)`：调用父类构造方法。  

当子类里写了同名成员（发生了字段隐藏）或者重写方法时，可以利用这个关键字访问父类的版本：

```java
class Person {
    protected String name = "person";
    public void sayHi() {
        System.out.println("Hi, I'm " + name);
    }
}

class Employee extends Person {
    protected String name = "employee"; // 字段隐藏，不推荐，但*可能会考*

    @Override
    public void sayHi() { // 方法重写
        System.out.println("Employee hi, " + name);
    }

    public void test() {
        System.out.println(name);       // employee（子类字段）
        System.out.println(super.name); // person（父类字段）

        sayHi();        // 调用到的是子类重写版本
        super.sayHi();  // 调父类版本
    }
}
```

顺带一提，字段是没有多态性的。

换言之，你的引用变量是什么类型，访问的就是谁的字段，不会有后面提到的动态绑定行为。

#### *构造方法链

在子类的构造方法的第一行，你只能做一件事：

* 要么 `super(...)`；
* 要么 `this(...)`（调用本类另一个构造方法）；
* 两者二选一，且必须是第一行；
* 最多只有一条这样的语句（不能既调用 `this()` 又调用 `super()` ）。

```java
class Person {
    Person(String name) { this.name = name; }
    protected String name;
}

class Employee extends Person {
    private int id;

    Employee(String name) {
        this(name, 0); // OK，第一行调用本类构造方法
    }

    Employee(String name, int id) {
        super(name);   // OK，第一行调用父类构造方法
        this.id = id;
    }
}
```

那如果不写会怎么样呢？编译器会帮你调用父类的默认/无参构造函数：

```java
class Person {
    Person() { System.out.println("Person()"); }
}

class Employee extends Person {
    Employee() {
        // 编译器会在这里偷偷塞一个super();
        System.out.println("Employee()");
    }
}
// 输出顺序如下，因为第一行里先调用了父类的构造函数
// Person()
// Employee()
```

如果父类**没有**无参构造，而你子类还不写 `super(...)`，编译器就会直接报错：

```java
class Person {
    Person(String name) { }
}

class Employee extends Person {
    Employee() {
        // 编译错误，编译器想塞 super()，但父类没有 Person()
    }
}
```

这里的要点是：**子类构造时，父类的构造函数必须先执行完。**那你可能会问，编译器都这样检查了，我为什么还得关注这个？我们看下面的例子，这其实是个多态性的坑：

```java
class Base {
    Base() { hook(); }      // 警告：在构造方法里调用了一个**可重写**方法
    void hook() { System.out.println("Base hook"); }
}

class Child extends Base {
    private String msg = "ready";

    @Override
    void hook() {
        System.out.println(msg); // 注意此处 msg 是“声明时初始化”
       // 可能是 null，因为子类实例字段尚未初始化
    }
}

// ...
Child obj = new Child();  // 语法不报错，可能输出 null（因为 msg 还没初始化到位）
```

构造 `Child` 对象时：

1. 先调用了 `Child` 的默认构造函数；
2. 编译器在该默认构造函数开头自动调用 `Base` 的无参构造函数；
3. Base 的无参构造函数调用了 `hook()` 这个可重写的方法；
4. 子类重写了该方法，调用子类的实现；
5. 此时程序还在父类构造过程中，**子类还没构造完**，`msg` 是默认值 `null`；
6. 输出了 `null`。

#### 继承结构中的构造顺序

在上一章里我们已经提到了实例/静态成员的构造顺序，这里我们用一个长一点的示例演示带上继承之后的构造顺序。概括来讲就是下面两种情况：

* 首次初始化——父静态 → 子静态 → 父实例 → 父构造 → 子实例 → 子构造；
* 后续初始化——父实例 → 父构造 → 子实例 → 子构造。

```java
class Base {
    static int bs = print("Base static field");
    static { print("Base static block"); }

    int bi = print("Base instance field");
    { print("Base instance block"); }

    Base() { print("Base constructor"); }

    static int print(String s) {
        System.out.println(s);
        return 0;
    }
}

class Child extends Base {
    static int cs = print("Child static field");
    static { print("Child static block"); }

    int ci = print("Child instance field");
    { print("Child instance block"); }

    Child() { print("Child constructor"); }

    static int print(String s) {
        System.out.println(s);
        return 0;
    }
}

public class Test {
    public static void main(String[] args) {
        System.out.println("main start");
        new Child();
        System.out.println("---- second ----");
        new Child();
    }
}
```

输出如下：

```plain
main start
Base static field
Base static block
Child static field
Child static block
Base instance field
Base instance block
Base constructor
Child instance field
Child instance block
Child constructor
---- second ----
Base instance field
Base instance block
Base constructor
Child instance field
Child instance block
Child constructor
```

### final 关键字

`final` 你前面其实见过了（常量那块），这里把和继承相关的三种再收个口：

* `final class`：该类不能被继承；  
* `final` 方法：该方法不能被覆盖；  
* `final` 变量：值不能再变（引用变量是“引用不变，对象可能还在变”）。  

```java
final class Utils { } // 不能 extends

class Base {
    public final void f() { }
}

class Child extends Base {
    // public void f() { } // 编译错误，final 方法不能重写
}
```

### 抽象类

**抽象类 (Abstract Class)** 你可以把它理解成“半成品类”：能放字段、能写普通方法，但它自己不能 new，而且通常会留一些“坑位”要求子类去填。它可以实现一种**“是什么”**的抽象。比如说下面的情况，你定义了表示图形的抽象类，但你定义 `Shape` 时也不知道它具体是个什么图形，但是你知道它肯定可以求面积，所以定义一个抽象方法，等待它的子类（也就是具体的图形）去实现。

```java
abstract class Shape {
    public abstract double area(); // 抽象方法：只有声明，没有实现
}

class Circle extends Shape {
    private final double r;
    public Circle(double r) { this.r = r; }

    @Override
    public double area() { // 实现抽象父类的方法
        return Math.PI * r * r;
    }
}
```

抽象类这一块你把下面几句背下来基本就够了：

* 抽象类**不能实例化**；  

```java
abstract class Animal {
    public abstract void speak();
}

// Animal a = new Animal(); // 编译错误：抽象类不能实例化
```

* 抽象方法**必须**在子类中实现（除非子类也声明成 abstract）；  

```java
abstract class Cat extends Animal {
    // 没实现 speak()，所以 Cat 也必须是 abstract，这样写合法
}

// class Dog extends Animal {}
// 报错，没有实现父类的方法
```

* 抽象类里可以有构造方法（子类用的，本身依然不能实例化）；  
* 抽象类也可以有普通方法和字段，这和后面要讲到的**接口**不同。

```java
abstract class Shape {
    protected double x, y;

    public Shape(double x, double y) {
        this.x = x;
        this.y = y;
        System.out.println("Shape constructor");
    }

    public abstract double area();
}

class Circle extends Shape {
    private final double r;

    public Circle(double x, double y, double r) {
        super(x, y); // 构造父类的部分，调用了抽象类的构造函数
        this.r = r;
    }

    @Override
    public double area() { // 实现抽象父类的方法
        return Math.PI * r * r;
    }
}

class Rectangle extends Shape { // Shape 的一个其他子类
    private final double w, h;
    public Rectangle(double w, double h) { this.w = w; this.h = h; }
    @Override public double area() { return w * h; }
}
```

### 对象转换与多态

**对象转换 (Casting)** 说白了就是“把引用当成另一种类型来看”。配合前面的方法重写，我们就会触及面向对象的另一大特性**多态 (Polymorphism)**，Java 里其实到处都有多态性的体现。

#### 向上转型

由于子类继承了父类的数据和行为，因此子类对象可以**安全地作为父类对象使用**，即子类对象可以自动转换为父类对象。可以将子类型的引用赋值给父类型的引用，也就是，在需要父类对象时，可以用子类对象替换，这也称为**里氏替换原则 (Liskov substitution principle，LSP)**。

```java
Shape s = new Circle(2.0); // Circle -> Shape，自动完成
System.out.println(s.area()); // 调的是 Circle.area()，这就是多态的核心
```

#### 向下转型

子类是它的父类的**特殊化**，每个子类的实例也都是它父类的实例，但反过来不成立。

所以，父类引用想变回子类，需要强转，而且可能会翻车：

```java
Shape s = new Circle(2.0);

Circle c1 = (Circle) new Rectangle(); // Rectangle 是 Shape 的一个其他子类，运行时抛异常
Circle c2 = (Circle) s; // OK，因为 s 里面真的是 Circle
```

如果你强转失败了，会在运行时发生异常：`ClassCastException`。

#### instanceof 关键字

为了避免“强转翻车”，可以先用 `instanceof` 检查它是不是某个类型的实例（对象）。instanceof运算符用来测试一个实例是否是某种类型的实例，这里的类型可以是类、抽象类、接口等：

```java
Shape s = new Circle(2.0);

if (s instanceof Circle) {
    Circle c = (Circle) s;
    System.out.println(c.area());
}
```

当然，更高的 Java 版本里支持这种语法糖：

```java
Shape s = new Circle(2.0);

if (s instanceof Circle c) { // 直接在这一行完成
    System.out.println(c.area());
}
```

#### *动态绑定

你看到 `s.area()` 调用到子类实现，这背后是**动态绑定 (Dynamic Binding)**——运行时根据对象**真实类型**决定调用哪个重写的方法。也就是说，调用方法时，真实调用到的方法**不取决于该引用变量的类型，而取决于该引用变量实际指向的对象类型**。

这也是为什么面向对象开发者很喜欢这样的写法：

```java
Shape[] shapes = { new Circle(1), new Circle(2) };
double sum = 0;
for (Shape sh : shapes) {
    sum += sh.area(); // 每个对象各自算自己的面积，而不需要你去写一大堆的转换和判断代码
}
```

然后我们来回收前面的伏笔，如果不写 `@Override` 注解，又刚好不小心写成重载了，会有什么行为：

``` java
class Parent {
    public void f() {
        System.out.println("Parent.f()");
    }

    public void g(int x) {
        System.out.println("Parent.g(int)");
    }

    public static void h() {
        System.out.println("Parent.h()");
    }
}

class Child extends Parent {

    // ① 真正的 Override：签名相同，运行时多态
    @Override
    public void f() {
        System.out.println("Child.f()");
    }

    // ② 这不是 Override，而是 Overload（重载）：参数不同
    // 如果你误加 @Override，编译器会报错拦住你
    public void g(double x) {
        System.out.println("Child.g(double)");
    }

    // ③ static 方法不会 Override，只会“隐藏”(hiding)
    public static void h() {
        System.out.println("Child.h()");
    }
}

public class Test {
    public static void main(String[] args) {
        Parent p = new Child();
        Child  c = new Child();

        // ===== ① Override：看对象真实类型（运行时） =====
        p.f(); // Child.f()
        c.f(); // Child.f()

        // ===== ② Overload：看引用静态类型 + 参数列表（编译期） =====
        p.g(1);     // Parent.g(int) （因为 Parent 里只有 g(int)）
        c.g(1);     // Parent.g(int) （1 是 int，优先匹配 g(int)，g(double) 不会“覆盖”它）
        c.g(1.0);   // Child.g(double)   （参数是 double，才选到子类重载）

        // ===== ③ static hiding：看引用静态类型（编译期） =====
        p.h(); // Parent.h()  （注意！不是 Child.h()）
        c.h(); // Child.h()
    }
}
```

#### 协变

我们在方法重写那一节还提到了返回类型的**协变性 (Covariant)**，这里也回收一下：

```java
class Person {
    public Person getSelf() {
        return this;
    }
}

class Employee extends Person {
    @Override
    public Employee getSelf() { // 返回类型更*具体*：Person -> Employee
        return this;
    }
}
```

稍加思考就知道这是合理的，因为 `Employee` 可以按之前的规则安全地转换为 `Person`，比如：

```java
Person p = new Employee();
Person p2 = p.getSelf(); // 编译期类型仍然是 Person，尽管事实上是 Employee

Employee e = new Employee();
Employee e2 = e.getSelf(); // 这里就能直接拿到 Employee
```

但是反过来让返回类型往父类走是不行的。对着上面的代码思考一下你应该可以自己弄明白。

### 自定义类库

所谓“自定义类库”，本质就是：把你写的一堆 `.class` 打包成一个 `.jar`，然后别的程序可以拿来用。

一个最小可用流程长这样：

* 写好类并放进合适的包里；  
* 编译得到 `.class`；  
* 打成 `.jar`；  
* 在另一个项目里把 jar 加进 classpath，再 import 使用。  

你不需要把每个命令背得滚瓜烂熟，但你至少要知道 jar 就是“装 class 的压缩包”，以及可执行 jar 可以用 `java -jar xxx.jar` 跑起来。感兴趣可以自己看这一章的 PPT。

---

## 写在上册结尾

到这里，Java 面向对象的基本特性我们就已经全部过完了。笔者希望你能够把到这里为止的内容理解透彻，因为这是面向对象的基础和核心内容。理解好了这些东西，后面的内容过起来会更轻松。

---

## 接口与内部类

从这章开始的内容会假定读者已经完全了解了之前的知识，所以我们会加快速度。

这一章的两个主角分别是“接口”和“内部类”。前者负责让你在单继承的世界里还能“像模像样”地复用行为；后者负责让你写出一些看起来很高级、但可能只是更难读的代码。

### *接口

**接口 (Interface)** 可以理解成一种“行为契约”，你只要承诺实现这些方法，你就**符合**（或者说**实现了**）这个接口。

它的几个默认规则：

* 接口里的方法**默认是** `public abstract`（所以写不写这俩关键字都一样）；  
* 接口里的字段**默认是** `public static final`（也就是之前讲过的**常量**）；  
* 一个类可以 `implements` 多个接口（这就是 Java 的“**曲线多继承**”）。  

不过，一般不推荐在接口里定义常量，因为使用**枚举**类型描述一组常量比接口中定义常量更好。后面会说。

一个最小接口（其实也有完全空的接口，我们后面会遇到）和使用长这样：

```java
interface Eatable {
    void eat(); // 等价于 public abstract void eat();
}

class Cat implements Eatable { // 实现 Eatable 接口
    @Override
    public void eat() { // 注意这里必须是 public
        System.out.println("Cat eats fish.");
    }
}
```

#### 接口的继承

接口之间用 `extends`，而且**可以多继承**（因为接口没有状态，继承起来没那么可怕）：

```java
interface A { void a(); }
interface B { void b(); }

// 接口可以同时继承多个接口
interface C extends A, B { }
```

当然，类也可以实现多个接口：

```java
class D implements A, B {
    @Override
    public void a() {
        System.out.println("D.a()");
    }

    @Override
    public void b() {
        System.out.println("D.b()");
    }
}

// 也可以直接实现继承了多个接口的 C（相当于同时实现 A 和 B）
class E implements C {
    @Override
    public void a() {
        System.out.println("E.a()");
    }

    @Override
    public void b() {
        System.out.println("E.b()");
    }
}
```

#### 接口类型的使用

接口**也可以当“引用类型”**来用，配合前面讲过的**多态**产生很多有用的特性：

```java
Eatable x = new Cat(); // 接口引用指向实现类对象
x.eat();               // 调用的是 Cat.eat()
```

这就是为什么很多 API 喜欢让你传接口而不是传具体类：它不关心你到底是 Cat 还是 Dog，只要你能 eat 就行。

比如我们可以写一个“只认接口”的排序方法—，它不关心数组里到底放的是什么类，只关心它们会不会“比较大小”。这是个最朴素的冒泡排序：

```java
static void bubbleSort(Comparable[] a) { // a 可以是任何 Comparable 
    for (int i = 0; i < a.length - 1; i++) {
        for (int j = 0; j < a.length - 1 - i; j++) {
            if (a[j].compareTo(a[j + 1]) > 0) { // 只调用接口方法
                Comparable tmp = a[j];
                a[j] = a[j + 1];
                a[j + 1] = tmp;
            }
        }
    }
}
```

你看这个参数类型：`Comparable[]`。它直接把“比较能力”写死成了一个接口要求。

所以你可以这么用：

```java
Integer[] a = { 3, 1, 4, 1, 5 }; // Integer 本身就实现了 Comparable
bubbleSort(a);
System.out.println(Arrays.toString(a));
```

这一段就是接口最喜欢干的事：**把“我需要什么能力”写清楚**，剩下的交给实现类去满足。

这里用到的 `Comparable`，我们会在本章末尾系统讲。

### 接口的非抽象方法

Java 8 之后接口里不只有抽象方法了。你会在接口里见到三种“有实现”的方法：

* **静态方法 (Static Method)**：直接写在接口里，靠接口名调用，和类的静态方法类似；  
* **默认方法 (Default Method)**：用 `default` 修饰，给实现类一个“默认实现”；  
* **私有方法 (Private Method)**：给接口内部复用代码用的，外面看不见。  

为什么要让接口能写实现？一个最现实的原因是：**接口要升级**。

假设你写了一个接口，而且已经有很多类实现了这个接口，此时你突然想给接口加一个新方法：

* 如果接口只有抽象方法，你一加编译器就会炸毛，告诉你**所有实现该接口的类都必须立刻补实现**；
* 默认方法就能让你“先给一个默认实现”，老代码不改也能编译，新代码再慢慢覆盖。

当然，实际工程中不建议你突发奇想给接口加方法。

#### 接口的静态方法

接口静态方法就当成“专属工具方法”理解就行：

```java
interface MathLike {
    static int add(int a, int b) {
        return a + b;
    }
}

int s = MathLike.add(1, 2); // 注意：只能用接口名调用
```

它不会像类静态方法那样被“继承”到实现类里，所以别写 `new Service().add(...)`，那是不存在的。

#### 接口的默认方法

如下所示，`Service` 类可以使用接口的默认方法，当然也可以像之前讲的那样重写：

```java
interface Loggable {
    default void log(String msg) {
        System.out.println("[LOG] " + msg);
    }
}

class Service implements Loggable {
    // 什么都不用写
}

public class Test {
    public static void main(String[] args) {
        new Service().log("started");
    }
}
```

接口的默认方法也可以被其他接口继承：

```java
interface A {
    default void f() {
        System.out.println("A.f()");
    }
}

interface B extends A {
    // 不写任何东西
}

class C implements B { }

public class Test {
    public static void main(String[] args) {
        new C().f(); // 输出 A.f()
    }
}

```

在上面那个例子里，如果子接口自己又有个同名的默认方法，那么父类的会被**覆盖**，有点像类的重写：

```java
interface A {
    default void f() {
        System.out.println("A.f()");
    }
}

interface B extends A {
    default void f() {
        System.out.println("B.f()");
    }
}

class C implements B { }

public class Test {
    public static void main(String[] args) {
        new C().f(); // 输出 B.f()
    }
}
```

如果一个类同时实现了两个接口，而这俩接口恰好有同名同签名的默认方法，那**编译器会逼你做选择**：你必须自己覆盖一次，不然它不知道该用谁的默认实现：

```java
interface A {
    default void f() {
        System.out.println("A.f()");
    }
}

interface B {
    default void f() {
        System.out.println("B.f()");
    }
}

// 编译错误，不知道用 A.f 还是 B.f
class C implements A, B {}
```

正确做法是你自己在类里“裁判”一下，甚至可以显式指定调用哪个接口的默认实现：

```java
class C implements A, B {
    @Override
    public void f() {
        A.super.f(); // 指定用 A 的默认方法
        // B.super.f(); // 也可以指定 B
    }
}
```

最后，**类中的方法永远优先于接口中的默认方法**，如下所示，`Base` 里的 `f()` 把接口 `A` 的默认方法覆盖掉了：

```java
interface A {
    default void f() {
        System.out.println("A.f()");
    }
}

class Base {
    public void f() {
        System.out.println("Base.f()");
    }
}

class C extends Base implements A { }

public class Test {
    public static void main(String[] args) {
        new C().f(); // 输出 Base.f()
    }
}
```

#### 私有方法

私有方法一般用来给接口内部的默认方法“提取公共步骤”，减少复制粘贴，外部不可见，下面是 PPT 上的例子：

```java
public interface MyInterface {
    static private void init2() { // 一个静态的私有方法
         System.out.println("静态私有方法");
     }
    public static void m() { // 在一个接口的静态方法中调用另一个私有方法
          init2();
     }
    private void init() { // 私有非静态方法
          System.out.println("完成某些初始化操作");
    }
   // 之前的知识应该已经足以让你明白
   // 这个 init 方法*只能*在这个 Interface 内部访问
   // 不需要被实现接口的类实现，也不能被它访问

    void normalInterfaceMethod(); // 这是一个普通的接口方法，要求实现它的类来实现
    default void defaultMethod() { // 默认方法
           init();
           init2(); // 默认方法里可以使用别的私有方法
    }

    default void anotherDefaultMethod() {
         init();
    }
}
```

上面的**私有方法只服务于接口内部**。实现类既不需要实现它，也不能调用它。

### 内部类

这玩意可能是个大考点，尤其是最后一节的匿名内部类。

**内部类 (Inner Class)** 就是“写在另一个类里面的类”。它主要用来：

* 把某些只服务于外部类的实现细节藏起来；  
* 方便访问外部类的成员（甚至 private 成员也能访问到）；  
* 写匿名类时少建文件（但可能会得到一坨**更混乱**的代码）。  

内部类有下面这几种常见的形态。

#### 成员内部类

这玩意在工程上用得很少，可读性比较差，而且容易造成生命周期耦合之类的问题。

它和外部类**对象**强绑定，所以你创建它时必须先有外部类对象：

```java
class Outer {
    private int x = 10;

    class Inner {
        void print() {
            System.out.println(x); // 能访问 Outer 的 private 字段
        }
    }
}

Outer outer = new Outer();
Outer.Inner inner = outer.new Inner();
inner.print();
```

PPT 上有另一种写法，本质上来说，想要创建 `Inner` 实例，必须先有一个 `Outer` 实例：

```java
var inner = new Outer().new Inner();
```

在使用成员内部类时需要注意下面几个问题：

* 成员内部类中**在 Java 16 以前不能**定义static变量和static方法，常量（`static final` 字段）除外；

* 成员内部类**可以**使用 abstract 和 final 修饰，其含义与其他类一样；

* 成员内部类**可以**使用 private、public、protected 或缺省修饰符，含义和普通类一样。
    区别在于你总是需要依赖于外部类的对象： `Outer.Inner i = outer.new Inner();`

其实，比较常见的情况是在**外部类的内部**创建它的实例，因为这时“外部类实例”是默认存在的。也就是说，你在外部类 `Outer` 的方法里可以直接：

```java
class Outer {
    class Inner { }

    void g() {
        Inner i = new Inner(); // OK：这里隐含了 Outer.this
    }
}
```

工程上相对常见的是使用 `private` 的成员内部类，内部类只作为实现细节，外部完全不能访问：

```java
class Outer {
    private class Inner {
        void help() {
            System.out.println("help");
        }
    }

    public void work() {
        Inner i = new Inner(); // OK，Outer 内部随便用
        i.help();
    }
}

// ...
Outer o = new Outer();
// Outer.Inner i = o.new Inner(); // 编译错误，Inner 不可见
```

#### 静态内部类

用 `static` 修饰的内部类，也称为 **嵌套类 (Nested Class)**。

它在工程上很常见，核心类库里就有不少（比如 `Map.Entry`、`Thread.State` 这种）。

静态内部类和成员内部类的关键不同点：

* 静态内部类中可以定义静态成员，而成员内部类不能；
* 静态内部类**只能**访问外层类的静态成员；
* 创建静态内部类实例**不需要先创建外层类实例**。

一个典型写法：

```java
class Outer2 {
    private static int x = 100;

    public static class Inner2 {
        private String y = "hello";

        public void innerMethod() {
            System.out.println("x is " + x); // 访问外层静态成员 OK
            System.out.println("y is " + y);
        }
    }
}

Outer2.Inner2 obj = new Outer2.Inner2();
obj.innerMethod();
```

你会发现它更像“放在类名空间下的一组工具类型”，不再背着一个外部对象到处跑。

#### 内部接口

你也可以在类里面定义接口（甚至接口里面再套接口，再套类……）。

有个很重要的规则：**内部接口的隐含属性是 static**。

换句话说，你就把它当成“写在**类名空间**下的一个接口类型”。

```java
class Outer3 {
    String s1 = "Hello";
    static String s2 = "World";

    interface MyInterface {
        void show();
    }

    static class Inner3 implements MyInterface {
        @Override
        public void show() {
            System.out.println("s1 = " + new Outer3().s1); // 访问实例成员得有对象
            System.out.println("s2 = " + s2);             // 静态成员可直接访问
        }
    }
}

Outer3.Inner3 inner3 = new Outer3.Inner3();
inner3.show();
```

多层嵌套在语法上当然能写，但你最好别在真实项目里乱搞嵌套极限运动。考试里遇到它，一般只是想让你认识语法并判断访问规则。

#### 局部内部类

局部内部类就是定义在方法体或语句块里的类，作用域非常小，有点“用完就扔”的意思。

它的特点：

* 不能写访问修饰符（因为它只在块里可见，写了也没意义）；
* 不能写静态成员（同样 `static final` 常量例外）；
* 可以访问外部类成员；
* 它内部的方法可以访问（或者说叫**“捕获”**）局部变量，但这些局部变量必须是“**有效 final (effectively final)**”，其实说人话就是，你没改过它，编译器就当它是 `final` 的。下面的例子：

```java
class Demo {
    void f() {
        int base = 10; // 没改过，所以是“有效 final”

        class Local {
            void run(int x) {
                System.out.println(base + x);
            }
        }

        new Local().run(5);
        // base++; // 你一旦改了 base，上面的 Local 就会被编译器拒绝使用 base
    }
}
```

你可能会问，为什么这里要求 `base` 不能变呢？首先我们要明确一点，局部内部类捕获的**不是“局部变量本身”，而是它的一个“值拷贝”**。因为 `base` 是一个局部变量，生命周期相当短（分配在栈上，离开作用域后就消失了），但内部类对象的生存周期很可能更长：

```java
// 考虑一个一般情况，比如在 f 中如此做：
Local l = new Local();
// 然后 l 可能被作为返回值传递给调用方法，或者被保存
// 这时，内部类对象 l 的生命周期*超过了*base
```

所以，当你让 `Local` 捕获了这个局部变量时，，其实 Java 编译器是这么处理它的：

```java
class Local {
    private final int baseCopy; // 拷贝进来

    Local(int baseCopy) {
        this.baseCopy = baseCopy;
    }

    void run(int x) {
        System.out.println(baseCopy + x);
    }
}
// 概念上是这样的结构，实际编译器实现可能更复杂
```

那么，就可以理解，假如你写了下面这样的代码：

```java
int base = 10;

class Local {
    void run() {
        System.out.println(base);
    }
}

base++; // 变成 11
```

那在一处地方使用 `Local` 对象的 `run()` 时，应捕获哪个 `base` 变量呢？所以，Java 从语言层面上禁止你这么做，从而消除了这个歧义。

当然，以上那一段你看不懂就算了，了解**被局部内部类捕获的局部变量必须是有效 final 的**即可。

#### *匿名内部类

匿名内部类最常见的用途是：临时实现一个接口/抽象类，写完用完就再也不想看到它第二次。

它的基本语法就是：

```java
new TypeName() {
    // 类体
}
```

比如你想临时写个多线程那一节里的 `Runnable`：

```java
Runnable task = new Runnable() {
    @Override
    public void run() {
        System.out.println("running...");
    }
};
new Thread(task).start();
```

匿名内部类的几个记忆点：

* 它**没有名字**，所以你**没法写构造方法**；
* 你定义的**同时就必须 new 一个实例**出来；
* 它不能同时“继承一个类并实现多个接口”（**顶多继承一个类或实现一个接口**）。

很多匿名内部类其实都能用 Lambda 表达式替代——前提是，目标类型是**函数式接口 (Functional Interface)**（也就是只有一个抽象方法的接口，比如 `Runnable`、`Comparator` 这种）。

同一个 `Runnable`，Lambda 表达式的版本**更短**也**更好读**：

```java
Runnable task = () -> {
    System.out.println("running...");
};
new Thread(task).start();
```

解释一下这个 `() -> { ... }` 的语法：

* `()` 是参数列表，这里 run() 没参数，所以是空括号；
* `->` 可以理解成“把参数映射到方法体”；
* `{ ... }` 是方法体。

如果方法体只有一行，花括号甚至可以省略：

```java
Runnable task = () -> System.out.println("running...");
```

Lambda 还有个非常舒服的用途：写 `Comparator` 时几乎不用再为“只比较一次”去建一个类了。

比如按字符串长度排序：

```java
List<String> list = Arrays.asList("aaa", "b", "cc");

// (a, b) 是 compare(a, b) 的两个参数
list.sort((a, b) -> a.length() - b.length());
```

有了这个铺垫，我们就可以很自然地聊聊：如果你想“比较对象大小”，到底应该用 `Comparable` 还是 `Comparator`。

### 比较对象的“大小”

你们大概不喜欢听离散数学那一套，所以我们说通俗一点。如果你对 C++ STL 的 `sort()` 比较了解，你可以尝试把这一节关联到“如何让对象能通过 `sort` 进行排序”上；如果不了解也没关系，根据前面的**接口**的那一套知识，我们可以定义一个接口，从而约定，实现该接口的对象**可通过某种自定义的方式“比较大小”**。

我们比较容易想到两条路：

* **自然顺序 (Comparable)**：让类自己实现 `Comparable<T>`，提供 `compareTo`；  
* **比较器 (Comparator)**：不改类本身，额外提供一个 `Comparator<T>` 来比较一种指定的对象。

这里可能涉及到一点泛型知识，不过我们可以姑且简单粗暴地理解为，这里的 `T` 是一个类型，表示实现 `Comparable` 的对象**能和哪种**对象“比大小”（一般是自己），或者 `Comparator` **能比较哪种**对象的“大小”。

#### Comparable

先来看 `Comparable` 接口。它只规定了一件事：**当前对象，如何和“另一个同类型对象”比较大小**。这个规则体现在 `compareTo` 方法的**返回值**上：

* 返回 **负数**：表示 `this` **小于** `other`
* 返回 **0**：表示两者 **相等**
* 返回 **正数**：表示 `this` **大于** `other`

你不需要纠结返回的是 `-1`、`1` 还是别的数字，**只要符号正确即可**。在实现时，我们通常**直接用数值相减或条件判断**来表达这种大小关系，而不是依赖包装类提供的现成方法，这样逻辑会更直观。

```java
class Employee implements Comparable<Employee> {
    private final int id;
    private final double salary;

    Employee(int id, double salary) {
        this.id = id;
        this.salary = salary;
    }

    @Override
    public int compareTo(Employee other) {
        // 按 id 升序
        if (this.id < other.id) return -1;
        if (this.id > other.id) return 1;
        return 0;
    }
}
```

定义好 `compareTo` 之后，对象之间就**天然具备了“大小关系”**。例如：

```java
Employee a = new Employee(1001, 8000);
Employee b = new Employee(1002, 9000);

System.out.println(a.compareTo(b)); // 负数，表示 a < b
```

更重要的是，一旦类实现了 `Comparable`，**所有基于“自然顺序”的排序工具就都能直接使用它**，比如：

```java
List<Employee> list = new ArrayList<>();
list.add(new Employee(3, 7000));
list.add(new Employee(1, 9000));
list.add(new Employee(2, 8000));

Collections.sort(list); // 按 id 升序排序
```

这里 `sort` 并不知道什么是 `Employee`，它只是**反复调用 `compareTo`**，把“谁大谁小”的决定权完全交给了你。

#### Comparator

有时候我们并不希望、也不方便修改类本身，这时就可以使用 **`Comparator`**。`Comparator` 的思想是：**“比较规则是外置的”**，在需要比较或排序时临时提供。

`Comparator` 的 `compare(a, b)` 方法，同样遵循和 `compareTo` 一样的返回值约定：

* 返回 **负数**：`a < b`
* 返回 **0**：`a == b`
* 返回 **正数**：`a > b`

最直观的写法，是直接用数值进行判断或相减，例如：

```java
// 按 salary 降序（谁钱多谁排前面）
list.sort((a, b) -> {
    if (a.getSalary() < b.getSalary()) return 1;
    if (a.getSalary() > b.getSalary()) return -1;
    return 0;
});
```

这样写可以帮助你**真正理解比较器的本质**。

我们只是明确告诉排序算法，“在我这里，**工资高的算‘更小’**，应该排在前面”。

在熟悉这种逻辑之后，就可以引入**核心类库中已经封装好的比较方法**，让代码更简洁、也更安全：

```java
// 使用 Double.compare，逻辑与上面完全一致
list.sort((a, b) -> Double.compare(b.salary, a.salary));
```

`Double.compare` 已经帮你处理了浮点数比较中的各种边界情况（例如精度和特殊值），但不管你用不用它，核心规则始终只有一条，即**比较方法返回值的正负号，决定了对象的“大小关系”**。

---

## 记录、枚举和注解类型

这一章本质上是一些语法糖和特殊的类。所以不需要有太大的心理压力。

> 语法糖：这种语法对语言的功能没有影响，但是**更方便**程序员使用。语法糖让程序更加简洁，有更高的可读性。语法糖通常是常见操作的简写，也可以用另一种更冗长的形式表达。
>
> 以上摘自维基百科。

### *记录类型

**记录 (Record)** 是从 Java 16 开始加入的一个新类型，它解决的是在写“**只负责装数据**”的类时写大量重复的、模板式的代码的问题。这是什么意思呢？

之前我们讲到，Java 的一种约定俗成的写法是让数据字段私有，然后以 getter/setter 方法访问这个字段。那么一种很常见的“只负责装数据”的类可能长下面这样，它本质上是**记录**了 `Customer` 对象的 `name` 和 `address` 信息，而且不允许这两个信息被修改，没有为它定义其他的行为（方法）：

```java
class Customer {
    private final String name;
    private final String address;

    public Customer(String name, String address) {
        this.name = name;
        this.address = address;
    }

    public String getName() { return name; }
    public String getAddress() { return address; }

    @Override public String toString() { /* 省略 */ return ""; }
    @Override public boolean equals(Object o) { /* 省略 */ return false; }
    @Override public int hashCode() { /* 省略 */ return 0; }
}
```

你会发现你写的大部分代码都在“重复劳动”：声明字段，然后写 getter，然后重写 `Object` 类的方法。

record 就是来治这个毛病的，上面一大坨类代码可以写成这样的记录：

```java
public record Customer(String name, String address) { }
```

编译器会自动帮你生成一堆东西：

* 所有字段都是 `private final`，对象创建后和 `String` 一样**不可变**；  
* 自动生成**全参构造方法**，就像你在记录的名字后面写的一样；  
* 自动生成 `equals()`、`hashCode()`、`toString()`，这三个东西的意义我们在介绍 `Object` 类的时候说；
* 每个字段都有一个同名访问方法，比如 `name()`、`address()`。  

另外下面给出一些编译器生成的 `equals()`、`hashCode()`、`toString()` 的规范，感兴趣可以细看一下。

> component 表示记录类的字段，也就是类名后面那个括号里的几个变量。
>
> 生成的 `equals` 必须满足：
>
> 1. **对方必须是“同一个 record 类”的实例**（同运行时类型）；
> 2. **每个 component 都要分别相等**，全部相等才返回 `true`。
>
> 更细一点：某个 component `c` 的“相等”判定规则是：
>
> - **如果 `c` 是引用类型**：按 `Objects.equals(this.c, other.c)` 来判等，注意，这允许 null 与 null 的比较，即 null == null 为真。
> - **如果 `c` 是基本类型**：使用对应包装类的 `compare` 来判等，即 `PW.compare(this.c, other.c) == 0`。例如 `int` 用 `Integer.compare`，`double` 用 `Double.compare`，`float` 用 `Float.compare`。
>
> 生成的 `hashCode` 必须保证：
>
> - 只要两个 record 的所有 component 值相同（从而 `equals` 为真），它们的 `hashCode` **也必须相同**。
>
> 生成的 `toString` 必须满足：
>
> - 输出字符串要包含：**record 类名**、**每个 component 的名字**、以及 component 值的字符串表示。
> - 还要求：如果两个 record `equals` 为真，则它们生成的字符串也应相同（极少数情况下，如果 component 自己的 `toString` 不稳定可能会“放宽”）。
> - **具体格式也是“可能变化的”**，Java 规范明确说：不要去解析这个字符串来反推出 component 值。

PPT 上使用记录类的一个例子：

```java
public class CustomerDemo {
    public static void main(String[] args) {
        var customer = new Customer("张明月","北京市海淀区");
        var customer2 = new Customer("李大海","上海市科技路20号");
        System.out.println("姓名：" + customer.name());
        System.out.println("地址：" + customer.address());		
        System.out.println(customer.toString());
        System.out.println(customer.equals(customer2));
        System.out.println(customer.hashCode());
        System.out.println(customer2.hashCode());
    }
}
// 输出：
// 姓名：张明月
// 地址：北京市海淀区
// Customer[name=张明月，address=北京市海淀区]
// false
// 2001326410
// 2109641328
```

记录类型还有几个你可能会被问到的性质：

* record 默认继承 `java.lang.Record`，不能显式继承别的类；  
* record 本身是 `final` 的，不能被继承；  
* record **可以实现接口**。  

你也可以在 record 里写自己的方法，就像使用普通的类那样：

```java
public record Point(int x, int y) {
    public int manhattan() {
        return Math.abs(x) + Math.abs(y);
    }
}
```

只是一般没必要这么做，记录就是用来单纯存放数据的。如果你需要写很多方法，你应该考虑使用普通的类。

### *枚举类型

**枚举 (Enum)** 解决的是“常量集合”的表达问题。

如果你不用枚举，你很可能会写出这种代码：

```java
class DirectionConst {
    public static final int EAST = 0;
    public static final int WEST = 1;
    public static final int SOUTH = 2;
    public static final int NORTH = 3;
}
```

它的问题是：**不安全**。

比如你可以把 999 也当成方向传进去，编译器不会拦你；你也可能把“星期的常量”和“方向的常量”搞混，反正都是 int，语法上都是合法的。

枚举的核心价值就是：把“取值范围”锁死，让错误尽量在编译期暴露。

一个最小枚举：

```java
enum Direction {
    EAST, WEST, SOUTH, NORTH
}
```

#### 枚举的基本用法

枚举最常见的搭配是 switch，因为它读起来非常像“规则表”：

```java
Direction d = Direction.WEST;

switch (d) {
    case EAST  -> System.out.println("→");
    case WEST  -> System.out.println("←");
    case SOUTH -> System.out.println("↓");
    case NORTH -> System.out.println("↑");
}
```

这比 switch(int) 的体验好得多，你不需要记住 0/1/2/3 分别代表啥，也不用担心会在 switch 里使用错误的值。

#### 枚举的方法

枚举**本质上是一个类**，它默认继承自抽象类 `java.lang.Enum`，因此有一些常用方法：

* `values()`：拿到所有枚举值的数组；
* `valueOf("EAST")`：从名字解析枚举；
* `name()`：枚举常量名（就是源码里写的那个标识符）；
* `ordinal()`：序号（从 0 开始，不建议当业务值）。

简单演示：

```java
for (Direction x : Direction.values()) {
    System.out.println(x.name() + " / " + x.ordinal());
}

Direction p = Direction.valueOf("EAST");
System.out.println(p);
```

`ordinal()` 最容易被滥用，千万不能随便使用这个值当作某种业务编码，不然你往枚举类里加一个字面值时可能就会引起一连串事故。

#### 像类一样工作

枚举不仅仅是“几个名字”。它也可以有字段、构造方法和普通方法。

最常见的写法是：给每个枚举值一个“业务字段”，比如中文名和编码：

```java
enum Color {
    RED("红", 1),
    GREEN("绿", 2),
    BLUE("蓝", 3);

    private final String cname;
    private final int code;

    Color(String cname, int code) {
        this.cname = cname;
        this.code = code;
    }

    public String cname() { return cname; }
    public int code() { return code; }
}
```

注意几个点：

* 枚举构造方法默认就是 private（你也只能写 private）；
* 枚举常量其实是**这个枚举类的几个固定实例**；
* 枚举也**可以实现接口**（比如你让枚举实现某个 `getCode()` 的接口，用于统一处理）。

### 注解类型

**注解 (Annotation)** 你可以把它当成“写在代码上的标签”。它本身不直接改变程序逻辑，但可以：

* 让编译器多做一些检查；
* 让工具/框架在编译期或运行时读取这些标签并做事情。

#### 注解的形态

注解按“有没有元素”大概分三种，元素的含义看后面的自定义注解那一节。

* 标记注解：没有元素，比如你只写 `@Override`；
* 单值注解：只有一个元素（通常叫 `value`），使用时可以**省略元素名，只写值**。；

比如像这样：

```java
@SuppressWarnings("unchecked")
public void f() {
    // ...
}
```

这里 `"unchecked"` 实际上是赋给了 `value` 这个元素，完整写法等价于：

```java
@SuppressWarnings(value = "unchecked")
public void f() {
    // ...
}
```

再举一个自定义注解的例子帮助你理解语法：

```java
@interface Author {
    String value();
}

@Author("Alice") // 在这里，"Alice" 赋值给了 value
class MyClass { }
```

* 普通注解：多个元素，要写 `name = value`。比如：

```java
@interface Info {
    String author();
    int version();
}

@Info(author = "Bob", version = 2)
class Demo { }
```

#### 内置注解

最常见的三个：

* `@Override`：告诉编译器“我在覆盖父类方法”，防止你手滑写成重载；
* `@Deprecated`：常见于公开类库中，告诉别人“这个东西快淘汰了，别用了”；
* `@SuppressWarnings`：压制某些警告，让编译器闭嘴（不要滥用！）。

`@Override` 我们在前面介绍方法重写时介绍过，如果你在重写时写错方法签名，它会直接让编译器报错：

```java
class Base {
    public void f() { }
}

class Sub extends Base {
    // @Override
    // public void F() { } // 大小写不对：这不是覆盖，是新方法
}
```

`@SuppressWarnings` 常见参数你见过就行，比如 `unchecked`、`deprecation`、`rawtypes` 等，考试一般考“认识它是干嘛的”。下面给你演示一下使用这玩意的语法：

```java
@SuppressWarnings(value={"unchecked","serial","deprecation"})
public class SuppressWarningDemo implements Serializable {
    public static void main(String[] args) {
        Date d = new Date();
        System.out.println(d.getDay()); 
      	// getDay 被 Deprecated 注解，本来会引起警告，但这里被我们抑制掉了
        List myList = new ArrayList();  // 该语句仍然有警告
        myList.add("one");
        myList.add("two");
        myList.add("three");
        System.out.println(myList);
    }
}
```

#### *自定义注解

你可以用 `@interface` 定义自己的注解类型，在注解类型中声明的方法称为注解类型的**元素**，它的声明类似于接口中的方法声明，**没有方法体，但有返回类型**。

```java
@interface Copyright {
    String value();          // 一个元素
    int year() default 2026; // 带默认值的元素
}
```

如果注解类型只有一个元素且它叫 `value`，可以用缩略写法：

```java
@Copyright("copyright 2010-2015")
class Demo { }
```

如果你希望“运行时还能读到这个注解”，你还需要用**元注解**（用来对注解进行注解的注解）描述它：

* `@Target`：这个注解能贴在哪（类上、方法上、字段上……）；
* `@Retention`：这个注解**保留到什么时候**（源码、编译后、运行时）。

你不需要把反射读注解写得多复杂，理解这个流程就够：

```java
import java.lang.reflect.Method;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
import java.lang.annotation.ElementType;

// 默认是RetentionPolicy.CLASS，保留到 .class 文件中，不带到运行时
// 所以我们必须手动指定这个注解的信息保留到运行时
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)           // 明确只能贴在*方法*上
@interface Tag {
    String value();
}

class A {
    @Tag("hot")
    public void f() { }
}

Method m = A.class.getDeclaredMethod("f");
Tag t = m.getAnnotation(Tag.class);
System.out.println(t.value());
```

考试大概率不会考到这么深，不过你至少得了解注解的信息是可以在运行时获得的。

---

## Java 核心类库

在搞定 Java 里的各种类结构之后，我们把视线投向 Java 内建的核心类库。

 Java 内建了很多类。它们大多在 `java.lang`、`java.math`、`java.time` 里，很多类你不用 import 也能用（至少 `java.lang` 是），比如 `String`，各种包装类 `Integer`, `Character`，在这一章我们具体地讨论它们的功能。

### *Object 类

正如我们在之前的继承那一章所说，所有类**最终都继承**自 `java.lang.Object`（哪怕你没写 `extends` 也会有隐式的继承）。所以 Object 的几个方法可以说是所有对象的“祖传方法”，有需要的时候你会重写它们。

#### toString

它用来**实现一个对象的字符串化表示**。不过，因为默认的实现拿不到什么运行时的对象信息，所以默认的 `toString()` 结果一般长得像 `com.example.Foo@1a2b3c`，信息量趋近于 0。想让对象的字符串化表示可读，就自己来重写。其实我们之前在讲 record 那一节的时候就说过，编译器会自动帮你重写记录类的 `toString()`。

```java
class Point {
    private final int x, y;
    Point(int x, int y) { this.x = x; this.y = y; }

    @Override
    public String toString() {
        return "Point{x=" + x + ", y=" + y + "}";
    }
}

System.out.println(new Point(1, 2));
// 输出：Point{x=1, y=2}
```

#### equals 和 hashCode

在 Java 里，引用类型的**判等**一直是个令人头疼的话题。默认 `equals()` 比的是“是不是同一个对象”（引用地址），这带来了很多问题。另外，很多时候我们希望从逻辑意义上比较两个对象是否相等。例如，两个记录对象的所有字段值都相同，我们就可以认为它们相等。

先把最容易考的点说清楚：

* 对基本类型来说，`==` 比的是值；
* 对引用类型来说，`==` 比的是“**是不是同一个对象**”（引用地址）；
* `equals()` 默认也等价于 `==`，但**很多类会重写它**，让它变成“比内容”，即“逻辑判等”。

也就是说，存在下面这种 `==` 和 `equals()` 不一致的情况，尽管我们直觉上认为两者应该完全等价：

```java
String a = new String("hi");
String b = new String("hi");

System.out.println(a == b);      // false：不是同一个对象
System.out.println(a.equals(b)); // true：内容相同
```

包装类和 String 都会重写 equals，所以你想做“值相等”，请养成用 `equals()` 的习惯。

另一个非常容易犯的错误是，用 `==` 去比包装类对象（尤其是 Integer），它可以把你坑到怀疑人生：

```java
Integer x = 127;
Integer y = 127;
System.out.println(x == y); // true（在缓存内）

Integer m = 128;
Integer n = 128;
System.out.println(m == n); // false！！不是哥们？（缓存范围外）
System.out.println(m.equals(n)); // true（值相等）
```

考试看到这类题，你只要记住，**包装类判等默认用 equals，不要赌它在不在缓存里**。

那 `hashCode()` 又是干嘛的？

核心直觉是：当你把对象放进 `HashMap / HashSet` 这种哈希容器时，它会先用 `hashCode()` 决定“你大概在哪个桶”，再用 `equals()` 决定“是不是同一个键/元素”。

所以有个硬约束：如果两个对象 equals 相等，那么它们的 hashCode **必须相等**。

反过来不要求：hashCode 相等**不一定** equals 相等（这叫**冲突**，允许存在，只要尽量少）。

因此，只要你要做“值相等”，就应该**同时覆盖** `equals()` 和 `hashCode()`：

```java
class Point {
    private final int x, y;
    Point(int x, int y) { this.x = x; this.y = y; }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof Point)) return false;
        Point p = (Point) o;
        return x == p.x && y == p.y;
    }

    @Override
    public int hashCode() {
        return 31 * x + y; // 一种简单的 hash 方法
    }
}
```

你写错 hashCode 的后果非常难以察觉，程序很少会崩，但 HashSet 的去重功能可能完全失效，HashMap 查找可能查不到，这在很多时候会是非常致命的。

PPT 上指出，在覆盖 `Object` 类的 `hashCode()` 时，可以~~偷懒~~直接联合类的每个实例变量的散列码。例如，下面是 `Employee` 类的 `hashCode()` 方法：

```java
import java.util.Objects; // 注意！这个不是 Object 是 Objects！要 import！

// Employee 类的定义自己脑补一下啦...
@Override
public int hashCode() {
    return Objects.hash(id, name, salary);
}
// java.util.Objects 类的 hash() 方法的参数是可变参数，
// 该方法计算每个参数的哈希码，并将它们组合起来。
// 这个方法是空指针安全的，也就是说里面的 name 是 null 也没问题。
```

另外，重写 `equals()` 时一定要注意，**参数是 `Object` 类型而不是当前类型**（比如这里的 `Point`）。

#### clone

`clone()` 考得多、用得少，属于“历史遗产”。

在说 clone 之前先把两个概念捋清：

* **浅拷贝**：复制对象本身的字段值，字段里如果是引用，就复制引用（两个对象会指向同一份子对象）；
* **深拷贝**：不仅复制对象本身，还要把**引用指向的子对象**也复制一份。

`super.clone()` 默认做的是浅拷贝。想用 clone 通常要满足：

* 类实现 `Cloneable`（不然会抛 `CloneNotSupportedException`）；
* 覆盖 `clone()`，并调用 `super.clone()`；
* 如果你有引用字段且想深拷贝，需要自己处理。、

#### 多线程相关的方法

PPT 上提到 `Object` 类还有这些和多线程相关的方法，我们留到多线程那一章再说：

```java
public void wait();
public void wait(long timeout);
public void wait(long timeout, int nanos);
// 使当前线程进入等待状态，直到另一个线程调用notify()或notifyAll()方法
 
public void notify();
public void notifyAll();
// 通知等待该对象锁的单个线程或所有线程继续执行
```

### *基本类型的包装类

为什么要引入包装类？因为很多地方不能用基本数据类型，而必须用“对象”：

* 泛型不支持基本类型（你不能写 `List<int>`）；
* 集合只能装引用类型（你不能往 `ArrayList` 里塞原始的 int）；
* 有时你需要用 null 表示“缺失值”（基本类型做不到）。

于是就有了 **包装类 (Wrapper Class)**：给基本类型套一层对象外壳。

八大基本类型对应包装类：

* `byte/short/int/long` → `Byte/Short/Integer/Long`；
* `float/double` → `Float/Double`；
* `char` → `Character`；
* `boolean` → `Boolean`。

**包装类对象是不可变的**：你“改”它，Java 其实偷偷创建了一个对象（和 String 很像），这一点我们在之前提过。

#### 自动装箱与拆箱

为方便基本类型和包装类型之间转换，Java 5版提供了一种新的功能，称为自动装箱和自动拆箱。

**自动装箱 (Auto-boxing)** 是指基本类型的数据可以自动转换为包装类的实例，**自动拆箱 (Auto-unboxing)** 是指包装类的实例自动转换为基本类型的数据。

```java
Integer a = 10; // 装箱：int -> Integer
int b = a;      // 拆箱：Integer -> int
```

别被“自动”坑了，如果拆箱的时候 `a` 是 null，会直接抛出空指针异常 (NPE)：

```java
Integer a = null;
int x = a; // 炸了！NullPointerException！
```

#### 数值类常量和创建方式

你会看到两种写法：

```java
Integer x1 = Integer.valueOf(10);
Integer x2 = 10; // 自动装箱，本质是偷偷用了 valueOf
```

从 Java 9 开始，Java 不推荐使用包装类的构造方法创建对象，所以不要写 `new Integer(10)`。

下面代码创建了几个Character对象并演示了有关方法的使用：

```java
var a = Character.valueOf('A');
var b = Character.valueOf('π');
char c = '中';   // c是char类型
System.out.println(a.compareTo('D')); // 输出：-3
System.out.println(Character.isJavaIdentifierStart(b)); 
// 是不是合法的 Java 标识符开头，输出：true
System.out.println(Character.isDigit(c));   
// 是不是数字，输出：false
```


#### 字符串与基本类型转换

当你从输入流之类的地方读入字符串后，你可能希望把它们解析为数字来处理，这些包装类提供了有关静态方法：

```java
int x = Integer.parseInt("123");        // "123" -> 123
double y = Double.parseDouble("3.14");  // "3.14" -> 3.14

String s1 = String.valueOf(456);        // 456 -> "456"
String s2 = Integer.toString(789);      // 789 -> "789"
```

另外，`Character` 的小工具方法也很爱考：

```java
System.out.println(Character.isDigit('7'));   // true
System.out.println(Character.isLetter('A'));  // true
System.out.println(Character.toLowerCase('Q'));
```

`Boolean` 你得知道它能从字符串中解析布尔值：

```java
System.out.println(Boolean.parseBoolean("true"));  // true
System.out.println(Boolean.parseBoolean("True"));  // true（忽略大小写）
System.out.println(Boolean.parseBoolean("yes"));   // false（不是 "true" 就当 false）
```

#### 数值包装类的使用方法

此外，诸如 `Integer` 一类的数值包装类提供了一系列实用静态方法：

```java
public static String toBinaryString(int i); // 返回i用字符串表示的二进制序列。
public static String toHexString(int i);    // 返回i用字符串表示的十六进制序列。
public static String toOctalString(int i);  // 返回i用字符串表示的八进制序列。
public static int reverse(int i);           // 返回将整数i的二进制序列反转后的整数值。
public static int signum(int i);       		  // 返回整数i的符号
public static int highestOneBit(int i);
// 返回整数i的二进制补码的最高位1所表示的十进制数。如7（111）的最高位的1表示的值为4。
public static int lowestOneBit(int i); 
// 返回整数i的二进制补码的最低位1所表示的十进制数。如10（1010）的最低位的1表示的值为2。

```

### Math 类

`java.lang.Math` 是典型工具类，在构造方法那一节我们就提到过，它通过将唯一的构造函数设为私有来避免你创建它的实例，然后提供一大堆静态方法用于数学计算。另外，它被标记为 `final`，无法被继承。

#### 几个常见的方法

典型的例子：

```java
double a = Math.sqrt(16);     // 4.0
double b = Math.pow(2, 10);   // 1024.0
int m = Math.max(10, 20);     // 20
double r = Math.random();     // [0, 1) 的随机数
```

当然，生成随机数我们一般用 `Random` 或 `ThreadLocalRandom`。感兴趣可以自行查询了解。

#### 舍入和取整

另外几个比较常见的，涉及到**舍入和取整**的：

```java
static double ceil(double x);    // 返回大于或等于x的最小整数
static double floor(double x);   // 返回小于或等于x的最大整数
static double rint(double x);    // 返回与x最接近的整数，如果x刚好卡在中间，返回其中的*偶数*
static int round(float x);       // 返回(int)Math.floor(x+0.5)，说白了就是四舍五入到整数
static long round(double x)      // 返回(long)Math.floor(x+0.5)，同上
```

#### *浮点数取小数点后若干位

另外，PPT 上有一个小例子演示了如何实现取浮点数的若干小数位：

```java
var pi = Math.PI;
pi = round(pi * 10000) / 10000.0; // 四舍五入到小数点后4位
```

### System 类

我们从 Hello world 开始就在使用 `System` 类了，它首先有这几个重要字段和方法：

* **`System.out`：标准输出**（本质是 `PrintStream`）；
* `System.err`：标准错误输出；
* **`System.in`：标准输入**，默认从键盘接受输入；
* `System.currentTimeMillis()` / `nanoTime()`：计时；
* `System.arraycopy(...)`：数组拷贝。

一个典型的计时写法，经常用来评估代码的性能：

```java
long t0 = System.nanoTime();
// ... 你要测的代码 ...
long t1 = System.nanoTime();
System.out.println("cost = " + (t1 - t0) + " ns");
```

### 高精度整数和浮点数

有时我们要处理的数字用基本的数据类型无法精确表示，因此 Java 为我们提供了 `BigInteger` 和 `BigDecimal`，前者解决“整数溢出”，后者解决“浮点不精确”。

#### BigInteger

```java
import java.math.BigInteger;

BigInteger a = new BigInteger("12345678901234567890");
BigInteger b = a.multiply(BigInteger.TWO);
System.out.println(b);
```

它们都用方法做运算，**没有 `+ - * /` 这种操作符重载**。~~想用操作符重载可以去写隔壁 C#。~~

#### BigDecimal

`BigDecimal` 最好的习惯：用字符串构造，别用 double 构造。

```java
import java.math.BigDecimal;

BigDecimal x = new BigDecimal("0.1");
BigDecimal y = new BigDecimal("0.2");
System.out.println(x.add(y)); // 0.3
```

### 日期与时间 API

这一章的细节建议自行翻阅 PPT，太细碎了，一股脑搬到这里来也没啥意思。

---

## 数组

数组是几乎所有编程语言都提供的一种数据存储结构。笔者把这一章放在整个手册相当靠后的部分，因为笔者认为数组的基本用法你应该都很了解，而数组作为**引用类型**的[特殊行为](###基本类型与引用类型)通过前面的阅读你应该已经相当熟悉了。

### 基本使用

好吧，我们还是必须先提一下它的基本使用方法。

#### 创建与初始化

数组的声明有两种写法：

```java
int[] a;  // 常见写法
int b[];  // 也能用，但不推荐（像隔壁 C 语言的遗迹）
```

在 Java 中创建数组需要 `new`：

```java
int[] a1 = new int[3];        // [0, 0, 0]
double[] a2 = new double[2];  // [0.0, 0.0]
String[] a3 = new String[2];  // [null, null]
```

也可以用初始化器语法：

```java
int[] a = { 3, 1, 4, 1, 5 };
```

数组有个天生字段：

```java
System.out.println(a.length); // 长度字段，不是方法，没有括号
```

数组下标从 0 到 `length - 1`，注意，在 Java 中访问越会抛出 `ArrayIndexOutOfBoundsException`，和 C 不声不响地返回意外的值/直接 SegFault 不太一样。

#### 遍历数组

其实在循环那一节我们就讲过了如何遍历数组。

* 普通 for：能拿到下标；
* 增强 for：更省事，但拿不到下标。

```java
int[] a = { 10, 20, 30 };

// 普通 for
for (int i = 0; i < a.length; i++) {
    System.out.println(i + ":" + a[i]);
}

// 增强 for
for (int x : a) {
    System.out.println(x);
}
```

如果你看着上面的增强 for 脑袋空空想不到可能遇到什么坑点的话，我建议你回头去看一下前面循环那一节。

### 从更高的视角看数组

你已经读到这里了，那么我希望你能够在看到数组时先问一问自己：

1. 数组是一个引用类型吗？—— **是**；
2. 如果是，那它是可变的还是不可变的？—— **可变的**；
3. 它存储在栈上还是堆上？—— **堆**上；
4. 它的父类是 `Object` 吗？—— **是**，但它自己**不是**一个独立的类。

你能够自己弄清上面这些问题，那它的几个坑点对你来说就不成问题了。

#### 长度不可变

相对于我们后面要提到的集合类，数组最重要的设计约束是：**数组的长度在创建后不可改变**。

我们总是在用数组的 `length` 字段，那你有没有考虑过，为什么我们用的不是 `getLength()` 这个 getter 方法？

答案是，它由 JVM 原生支持，**直接**由 JVM 负责分配、访问和边界检查，`length` 这个字段其实也是 JVM 直接提供的元数据。它并**不存在于 Java 源码的类继承体系**中，所以没有一个 “class” 的地方能为它定义 getter 方法。

#### 数组作为参数与返回值

如果忘记了的话，可以读方法的参数传递那一章。

```java
static void fillZero(int[] a) {
    for (int i = 0; i < a.length; i++) a[i] = 0;
}

int[] a = {1, 2, 3};
fillZero(a);
System.out.println(Arrays.toString(a)); // [0, 0, 0]
```

#### 数组复制

不像基本数据类型，你不能通过一次赋值来直接复制一个数组的内容：

```java
int[] a = {1, 2, 3};
int[] b = a; // 指向同一个数组对象
```

除了手动创建新的等长数组、然后用循环把元素一个一个复制过去之外，我们还有这些办法：

* `System.arraycopy`：底层方法，**JVM 优化**，参数多但性能非常好；

```java
int[] src = { 10, 30, 20, 40 };
int[] dst = new int[src.length];
System.arraycopy(src, 0, dst, 0, src.length);
```

* `Arrays.copyOf` / `copyOfRange`，借助马上要说到的 `Arrays` 工具类，语义更直观；

```java
int[] b = Arrays.copyOf(a, a.length);
```

* `clone()`：浅拷贝，写起来最短。

```java
int[] b = a.clone();
```

需要注意的是，以上几种方法**本质上都是浅拷贝**。

#### *可变参数

可变参数本质上是个简单的语法糖。你写：

```java
static int sum(int... nums) {
    // 在方法内部，nums 就是一个 int[]
    int s = 0;
    for (int x : nums) s += x;
    return s;
}

// 在调用者看来，sum 方法可以接受任意个 int 参数
System.out.println(sum(1, 2, 3));
System.out.println(sum()); // 也可以传 0 个
```

编译器会帮你把 `sum(1,2,3)` 变成 `sum(new int[]{1,2,3})`。

需要注意的是，这样的**可变参数必须在参数列表的最后一位**。如果它后面还能再跟参数，编译器将无法判断哪些实参属于这个数组。

#### 可变参数与重载

带有可变参数的方法在重载时可能会有一些坑。方法重载解析时，它的优先级**最低**，这很容易造成“看起来能调用，实际上调的不是你想的那个方法”。比如说下面的几个例子：

```java
// 1. 最基本的情况
static void f(int x) {
    System.out.println("int");
}
static void f(int... x) {
    System.out.println("varargs");
}
f(10); // 输出：int

// 2. 可以自动装箱的情况
static void g(Integer x) {
    System.out.println("Integer");
}
static void g(int... x) {
    System.out.println("varargs");
}
g(10); // 输出：Integer

// 3. 传入 0 个参数的情况
static void h(int... x) {
    System.out.println("varargs");
}
static void h() {
    System.out.println("no args");
}
h();   // 输出：no args
```

所以，只有当**没有更具体的方法可用**时，编译器才会选择带可变参数的重载版本。

#### 二维数组

二维数组本质上是“**数组的数组**”，所以它既可以是“规整”的，也可以是“参差不齐”的。

“规整”的二维数组：

```java
int[][] m = new int[2][3]; // 2 行 3 列
m[0][1] = 7;
```

“参差不齐”的二维数组，每行长度可以不一样：

```java
int[][] jagged = new int[3][];
jagged[0] = new int[1];
jagged[1] = new int[3];
jagged[2] = new int[2];
```

你把它当成“每一行都是一个独立的一维数组”就不会困惑了。

二维数组最经典的案例是矩阵乘法（考试常考思路）：

```java
// A: r x k, B: k x c, 结果 C: r x c
static int[][] mul(int[][] A, int[][] B) {
    int r = A.length;
    int k = A[0].length;
    int c = B[0].length;

    int[][] C = new int[r][c];
    for (int i = 0; i < r; i++) {
        for (int j = 0; j < c; j++) {
            int s = 0;
            for (int t = 0; t < k; t++) {
                s += A[i][t] * B[t][j];
            }
            C[i][j] = s;
        }
    }
    return C;
}
```

另外，在这种情况下数组的每个元素（也是数组）**也具有引用类型的性质**，在复制时需要注意。笔者相信读到这里的你已经完全明白我在说什么了。

### Arrays 工具类

`java.util.Arrays` 是处理数组的瑞士军刀，用来弥补数组本身缺失的功能，常用方法记住这几个：

* `toString` / `deepToString`：打印数组，后者一般用于二维数组；
* `sort`：排序，**默认升序**排序；
* `binarySearch`：二分查找（必须是已排序的数组）；
* `equals` / `deepEquals`：比较数组内容；
* `fill`：用一个值填满数组。

```java
import java.util.Arrays; // 使用 util 里的类时记得 import！

int[] a = { 3, 1, 4, 1, 5 };

Arrays.sort(a);
System.out.println(Arrays.toString(a));

int idx = Arrays.binarySearch(a, 4);
System.out.println(idx); // 二分查找指定元素的下标

System.out.println(Arrays.equals(new int[]{1,2}, new int[]{1,2})); // 逻辑判等
Arrays.fill(a, 7);
System.out.println(Arrays.toString(a));

// 运行结果：
// [1, 1, 3, 4, 5]
// 3
// true
// [7, 7, 7, 7, 7]
```

对象数组排序（和二分查找）要么元素实现 `Comparable`，要么你传一个 `Comparator`（在接口那章讲过了）。

### 冒泡排序

Java 应该不至于考这个。如果你要看的话前面比较对象的大小那一节也有。

---

## 字符串

字符串可能是 Java 里最常用的引用类型。它用得太多了，以至于 JVM 专门给它开了“加速通道”（常量池、拼接优化等等，感兴趣自行了解）。

### String 基本操作

`String` 是引用类型，但正如[之前](####成员访问)所提，它有一个核心性质：**不可变**。你对它做的任何“修改”，本质都是创建了一个新对象。

```java
String s = "Hello";
s.replace('o', 'A');          // 注意 replace 方法是返回替换完成后的字符串
System.out.println(s);        // 还是 Hello

s = s.replace('o', 'A');      // 这才是真的“改了”
System.out.println(s);        // HellA
```

常用方法不用背一百个，掌握这些就够：

* `length()`：**长度**，注意和数组不一样，`String` 的长度是**方法**；
* `charAt(i)`：取下标为 i 处的字符；
* `substring(a, b)`：取下标 a 到 b 的子串（左闭右开）；
* `indexOf` / `contains`：查找指定子串；
* `replace` / `toUpperCase`：生成新字符串。

#### 字符串比较

在 Java 核心类库的 `Object` 一节我们已经重点讨论了引用类型判等的问题：

> `==` 判引用相等，`equals()` 判逻辑相等。

但在 String 这里还不够，因为 **JVM 里有字符串常量池**会坑你：

```java
String a = "Hello";
String b = "Hello";
System.out.println(a == b);       // true（同一个常量池对象）

String c = new String("Hello");
System.out.println(a == c);       // false（new 了一个新对象）
System.out.println(a.equals(c));  // true（内容相同）
```

所以考试里只要看到“字符串比较”，你默认用 `equals()`，用 `==` 的结果你基本是没法预测的。

另外 `compareTo` 也常出现：它**按字典序比较**（返回值语义和 Comparable 相同）：

```java
System.out.println("abc".compareTo("abd")); // 负数
System.out.println("abc".compareTo("abc")); // 0
System.out.println("abd".compareTo("abc")); // 正数
```

### 字符串查找与匹配

常见需求：

```java
String s = "hello world";

System.out.println(s.indexOf("lo"));    // 3
System.out.println(s.contains("or"));   // true
System.out.println(s.startsWith("he")); // true
System.out.println(s.endsWith("ld"));   // true
```

如果题目突然冒出 `matches`，那它用的是**正则表达式**（不用精通，但要知道它不是“普通包含”）：

```java
System.out.println("123".matches("\\d+")); // true
System.out.println("12a".matches("\\d+")); // false
```

### *字符串拆分与组合

我们可能经常需要按某个特定分隔符拆分字符串，这时候可以用 `split`；将多个字符串拼起来时不用自己手写循环，可以直接用 `String.join`：

```java
String line = "a,b,c";
String[] parts = line.split(","); // 按 "," 这个子串切成数组
System.out.println(Arrays.toString(parts));

String again = String.join("-", parts); // 用 "-" 连接
System.out.println(again);
```

把字符串转成字符数组也很常见：

```java
char[] cs = "abc".toCharArray();
System.out.println(Arrays.toString(cs));
```

### 文本块

从 Java 15 开始，你可以用 **文本块 (Text Block)** 写多行字符串：

```java
String poem = """
白日依山尽，
黄河入海流。
欲穷千里目，
更上一层楼。
""";
System.out.println(poem);
```

开头三个引号后面通常要换行，紧跟在三个引号 `"""` 后面的**换行符会被自动忽略**。但注意这里结尾处 `"""` 前的换行符是**有效的**。

### 命令行参数

`main` 方法的 `args` 本质是一个 `String[]`，考试应该不至于考命令行传参的问题：

```java
public static void main(String[] args) {
    // java Demo Alice 18
    String name = args[0];
    int age = Integer.parseInt(args[1]);
    System.out.println(name + ":" + age);
}
```

### 格式化输出

你可以用 `printf` 或 `String.format`，学过 C++ 的同学应该能感觉到家的味道：

```java
int id = 7;
double score = 98.5;
// 这里的 %n 表示一个跨平台的换行符，在 Windows 上解析为 \r\n，在 macOS / Linux 上解析为 \n
System.out.printf("id=%d, score=%.1f%n", id, score);
```

### *StringBuilder

`String` 不可变，所以在循环里用 `+` 拼字符串会产生一堆临时对象。此时请用 `StringBuilder`：

```java
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 3; i++) {
    sb.append(i).append(","); // 链式调用，其实是 Builder 设计模式
}
String result = sb.toString();
System.out.println(result);
```

`sb.append()` 方法会返回当前 `StringBuilder` 对象本身，这让你可以使用上面那种链式调用的语法。

`StringBuffer` 是线程安全版本，题目没提并发的话默认就用 `StringBuilder` 就行。

其实你写 `"a" + b + c` 的时候，编译器很多时候会偷偷帮你换成用 StringBuilder 拼。

### 字符串经典案例

回文串字符串可以看作字符数组，用 `charAt()` 方法访问指定下标处的字符：

```java
static boolean isPalindrome(String s) {
    int i = 0, j = s.length() - 1;
    while (i < j) {
        if (s.charAt(i) != s.charAt(j)) return false;
        i++;
        j--;
    }
    return true;
}
```

#### 字符串加密解密

教学示例里常用“凯撒移位”或“异或”。这里给一个凯撒移位的例子：

```java
static String caesar(String s, int k) {
    StringBuilder sb = new StringBuilder(s.length());
    // 处理负数偏移，确保 k 在 0-25 之间
    k = (k % 26 + 26) % 26; 

    for (char ch : s.toCharArray()) {
        if (ch >= 'a' && ch <= 'z') {
            // (当前位置 + 偏移) % 26 + 基准
            sb.append((char) ((ch - 'a' + k) % 26 + 'a'));
        } else if (ch >= 'A' && ch <= 'Z') {
            sb.append((char) ((ch - 'A' + k) % 26 + 'A'));
        } else {
            sb.append(ch); // 非字母原样输出
        }
    }
    return sb.toString();
}
```

#### 十进制转二进制字符串

从当年学 C++ 的时候陪伴你到现在了，求求你别告诉我你不会这个了好不好：

```java
static String toBinary(int n) {
    if (n == 0) return "0";

    StringBuilder sb = new StringBuilder();
    while (n > 0) {
        sb.append(n % 2);
        n /= 2;
    }
    return sb.reverse().toString();
}
```

字符串这一章的主要重点是：不可变、equals、StringBuilder。其他内容你见多了自然就会了。

---

## 泛型与集合

泛型在 C++ 里可以是相当深奥的话题。我们在这一章只介绍基本的 Java 泛型用法。

* **泛型 (Generics)**：让**“类型参数化”**，把类型错误尽量提前到编译期；
* **集合框架 (Collections Framework)**：一套标准容器接口与实现，帮你少造轮子。

### *泛型

#### 为啥要有泛型

先看一个没有泛型的例子：

```java
List list = new ArrayList(); // 没写泛型 -> 原始类型，丸辣！
list.add("hello");           // 先塞了个 String
list.add(123);               // 你居然还能往同一个 List 里塞 int 的包装类

String s = (String) list.get(1); // 运行时爆炸：ClassCastException
```

泛型的价值就是把这种错误提前到编译期：

```java
List<String> list = new ArrayList<>();
list.add("hello");
// list.add(123); // 编译器直接拒绝：类型不匹配

String s = list.get(0); // 不需要强转
```

#### 泛型类

泛型类的基本语法是在类名后面加一对尖括号，里面写**泛型参数**，可以有一个或多个。

按照约定，类型参数名使用单个大写字母表示。常用的类型参数名有：E 表示元素，K 表示键，N 表示数字，T 表示类型，V 表示值等。

```java
class Box<T> {
    private T value; // T 是一个”类型参数“
    public void set(T v) { value = v; }
    public T get() { return value; }
}

Box<String> b = new Box<>(); // 把 T 全部”换成“String
b.set("hi");
String s = b.get();
```

注意限制：泛型参数**不能是基本类型**，所以 `Box<int>` 不行，要写 `Box<Integer>`。

#### 泛型接口

接口也能带类型参数。马上在集合的 Map 那里就要用，`Map<K, V>` 就是“键类型 + 值类型”。

下面这个可以表示一个泛型的键值对：

```java
interface Pair<K, V> { // 多个泛型参数
    K key();
    V value();
}
```

#### 泛型方法

方法自己也能声明类型参数，从而使其能灵活地处理不同类型的参数

```java
static <T> void print(T x) {
    System.out.println(x);
}

print(123);
print("abc");
```

#### 通配符

在 PPT 上这块讲得比较混沌，就罗列了一下不同的语法。笔者在这里借助 **PECS 规则**来帮助你理解。

如果你弄懂了之前面向对象特征那一章的继承特性，那么顺着把这一小节读完应该就足够你弄清楚通配符和有界类型参数的问题了。

Java 泛型里最容易让人困惑的两点是：

1. 泛型是**不变的 (Invariant)**：`List<Integer>` **不是** `List<Number>` 的子类型。
2. 我只想读/只想写/读写都要：这正对应了 `? extends`、`? super` 和“精确类型参数”。

**不变性**会导致这么个问题：

```java
List<Integer> ints = List.of(1, 2, 3); // 这是一个 List<Integer>

// List<Number> nums = ints; // 编译失败：List<Integer> *不是* List<Number>
```

其实这里你并不一定是想把它转换成 `List<Number>` 来同时进行读写，按读写两种情况，我们分开来看。

通配符 `?` 在这里解决的问题是，**使用已经建好的泛型对象**时，能不能适当“放宽”泛型限制。

第一种情况，你可能只是想把它**当成“某种 Number 的列表”来读**。下面是个具体的例子：

```java
static double sum(List<? extends Number> list) {
    double s = 0.0;
    for (Number n : list) {
        s += n.doubleValue(); // 想读取 list 的元素，看成 Number 取 double 值，安全
    }
    return s;
}

List<Integer> ints = List.of(1, 2, 3);
List<Double> doubles = List.of(1.5, 2.5);
double a = sum(ints);     // OK
double b = sum(doubles);  // OK
```

这里的 `list` 是一个“**生产者 (Producer)**”，它“**生产**”出元素供 `sum()` 方法“**使用**”， `? extends Number` 这个类型上界确保了元素“**可用**”，这个规则称为 **“Producer Extends (PE)”**。

另一种情况，我们可能是把它**当成“某种 Number 的列表”来写**，也给个具体的例子：

```java
static void addSomeIntegers(List<? super Integer> list) {
    list.add(1);
    list.add(2); // 想要往 list 里写入元素，list 的类型肯定可以“装”得住 Integer，安全
  
  	// 如果要读的话必须使用 Object，不是类型安全的
}
List<Number> a = new ArrayList<>();
addSomeIntegers(a);
```

这时候 `list` 是一个“**消费者 (Consumer)**”，它要“**吃掉**”你的方法里“**生产**”的元素（在这里就是 `1` 和 `2`），所以这个 `list` 得有个“大胃袋”，能**安全地装下**你生产的数据，`? super Number` 这个类型下界确保了元素“**能放进 `list`**”，这个规则称为 **“Consumer Super (CS)”**。

以上两条规则就是 Google 的前首席 Java 架构师 Joshua Bloch 总结的 **PECS 规则**：

> *Producer Extends, Consumer Super.*

看得懂下面这个“魔法”你的理解就完全到位了：

```java
static <T> void copy(List<? extends T> src, List<? super T> dst) {
  	// 把所有 src 的元素复制到 dst
  	// src 只读不写，是 Producer，用 ? extends T，保证它的元素至少能当作 T
  	// dst 只写不读，是 Consumer，用 ? super T，保证它吃得下
    for (T t : src) {
        dst.add(t);
    }
}
```

如果是同一个 `list` 读写兼有的情况，这里就必须用精确的类型（相当于被上下界卡死了）。

#### 有界类型参数

有界类型参数解决的是**定义新的泛型方法/泛型类**时需要该类型参数具有特定功能的问题。

比如说，定义 `max()` 这个方法，要求 `T` 至少得能比大小：

```java
static <T extends Comparable<T>> T max(T a, T b) {
    return a.compareTo(b) >= 0 ? a : b;
}
```

如果这里不写这个 `extends Comparable<T>` ，编译器就没法保证 `a` 有 `compareTo()`，从而报错。

另外有界类型参数也可以同时要求继承自某个类且实现了某个接口：

```java
static <T extends Number & Comparable<T>> T maxNumber(T a, T b) {
    return a.compareTo(b) >= 0 ? a : b;
}
```

还有一种通配符无法解决的情况，这里的 `T` 事实上约束了 `value` 和 `list` 的元素必须是同一类型的：

```java
static <T> boolean allEqual(List<T> list, T value) {
    for (T t : list) {
        if (!t.equals(value)) {
            return false;
        }
    }
    return true;
}

```

#### 类型擦除

泛型在编译后会进行 **类型擦除 (Type Erasure)**，例如，运行拿不到 `List<String>` 这种信息，只剩 `List`。这里的 `List` 称为**源类型**，`List<String>` 称为**参数化类型**，这导致两类常见限制：

一是不能做**参数化类型**的 instanceof：

```java
List<String> a = new ArrayList<>();
// if (a instanceof List<String>) { } // 编译错误，运行时已经没有 List<String> 了
```

二是**不能直接 new 泛型数组**：

```java
// T[] a = new T[10]; // 编译错误！在 C++ 里是可以这么做的
```

这也是 Java 经常被人们所诟病的一点。不过在我们考试的场景里，理解泛型是“编译期类型检查工具”就足够了。

### 集合

对于学过数据结构和用过 C++ STL 的你来说这一节应该挺简单的。既然我们已经学完了前面的接口、泛型的东西，我们就直接从最基本的 `Collection` 接口和 `Map` 接口开始梳理这一部分。

#### Collection 家族

##### 可迭代接口

让我们先看看集合的最初本源：`Iterable<E>` 接口。

```java
public interface Iterable<E> {
    Iterator<E> iterator();
}
```

它的定义十分简单，要求实现类给出一个**迭代器**。实现了该接口的类就可以通过增强 for 循环来遍历：

```java
for (E e : iterableObject) {
    // ...
}
```

> 为什么集合要单独定义一个“可迭代”的接口而不是直接用数组作为底层的结果呢？因为集合中的元素不像数组一样在内存空间中连续存放，而且**长度可变**，因此考虑这个“可迭代”时你可以在脑子里构想一个单向链表，它通过循环地访问当前 Node 的下一个 Node 来实现对链表的遍历，直到下一个 Node 为 null。

需要注意的是：

- `Iterable` 不关心元素存在哪；
- 也**不关心是否支持增删**；
- 它只约定实现该接口的类**能不能遍历其元素**。

##### 集合接口

在这个基础上，我们可以定义集合接口：

```java
public interface Collection<E> extends Iterable<E> {
    boolean add(E e);            // 可以添加元素
    boolean remove(Object o);    // 可以删除元素
    boolean contains(Object o);  // 可以查询元素是否在集合中
    int size();                  // 可以获取集合的大小（元素个数）
    boolean isEmpty();           // 可以判断集合是否为空
    void clear();                // 可以清空
    // ...
}
// 根据接口的性质，后面那些实现类全部*都有*这些方法。
```

从这些方法可以看出，`Collection` 关心的是：

- 元素的**添加 / 删除**；
- 元素是否在集合中；
- 集合的规模。

但它并没有规定：元素是否有顺序，是否允许重复，是否能按下标随机访问。

##### List、Set、Queue 概述

这些规则就由更低一层的接口来定义。`Collection` 接口的子接口有：`List`、`Set`、`Queue`。

* `List` 接口要求元素**有顺序**、**可重复**、**可按下标访问**；
    * 类 `ArrayList` 的实现：基于数组，随机访问快；
    * 类 `LinkedList` 的实现：基于链表，插入删除更灵活；
* `Set` 接口要求元素**不可重复**，不可按下标访问，是否有序得看具体实现；
    * `HashSet`：基于哈希表，无序，依赖 `equals/hashCode`，判断存在性复杂度可达到 O(1)；
    * `TreeSet`：基于红黑树，有序，依赖 `Comparable` 或 `Comparator`，哈希碰撞频繁时性能更优；
    * 注意，`Set` 接口实现的 `add()` 方法在**添加重复元素时会返回 `false`**。
* `Queue` 接口描述了按**特定顺序处理元素**的集合；
    * 常见的实现类是 `ArrayDeque`。

集合是**可变的**，你把集合传给别人，别人就能改你的集合，和数组是类似的。

下面演示一些常见的集合类使用。

##### List

总体上来说这玩意比较像 C++ 的 vector：

```java
List<String> list = new ArrayList<>();
list.add("a");
list.add("b");
list.add("a");

System.out.println(list.get(0)); // a
System.out.println(list);        // [a, b, a]
```

`List` 在 `Collection` 的基础上新增了：

```java
E get(int index);                // 获得指定下标的元素
E set(int index, E element);     // 设定指定下标的元素
void add(int index, E element);  // 在指定下标插入元素
```

##### Set

去重，做集合运算的时候很方便：

```java
Set<String> set = new HashSet<>();
set.add("a");
set.add("b");
set.add("a"); // 注意这里加入重复元素时 add 其实返回了 false

System.out.println(set); // [a, b]

```

##### Queue

数据结构里已经熟得不能再熟了：

```java
Queue<String> q = new ArrayDeque<>();
q.offer("A");
q.offer("B");

System.out.println(q.poll()); // A
System.out.println(q.peek()); // B
```

#### Map 家族

`Map` **并不**派生自可迭代。它自成一派：

```java
public interface Map<K, V> { // 原始定义不长这样！此处仅用于帮助理解！

    // 基本操作
    int size();					// 大小
    boolean isEmpty();  // 是否为空

    boolean containsKey(Object key);      // 是否包含某个键
    boolean containsValue(Object value);  // 是否包含某个值

    V get(Object key);     // 按键查值
    V put(K key, V value); // 插入新的键值对
    V remove(Object key);  // 按键删除指定键值对

    void clear();	 // 清空 Map

    // 视图方法，将它当成别的集合类型来看
    Set<K> keySet();
    Collection<V> values();
    Set<Map.Entry<K, V>> entrySet();

    // 内部接口：键值对
    interface Entry<K, V> {
        K getKey();
        V getValue();
        V setValue(V value);

        boolean equals(Object o);
        int hashCode();
    }
}

```

特点：**键唯一**，值随意，存储键到值的**映射关系**，并且能高效地用键查询值。

常用实现类：

* `HashMap`：无序，平均查询复杂度 O(1)，允许一个 null 键；
* `TreeMap`：按键排序，平均查询复杂度 O(log N)，不允许 null 键/值；
* `Hashtable`：老古董，线程安全但一般不推荐新代码用，它也不允许 null 键/值。

```java
Map<String, Integer> map = new HashMap<>();
map.put("a", 1); // 用 put 方法插入键值对
map.put("b", 2);

System.out.println(map.get("a")); // 用 get 方法查询，得到 1
```

遍历最常用的是 `entrySet()`：

```java
Map<String, Integer> cnt = new HashMap<>();
cnt.put("a", 1);
cnt.put("b", 2);

for (Map.Entry<String, Integer> e : cnt.entrySet()) { // 这里的 e 就是键值对
    System.out.println(e.getKey() + " -> " + e.getValue());
}
```

你可能也会用到键的集合 `keySet()` 和值的集合 `values()`：

```java
for (String k : cnt.keySet()) { /* ... */ }
for (Integer v : cnt.values()) { /* ... */ }
```

#### Collections 工具类

`java.util.Collections` 类似 `java.util.Arrays`，是集合的工具箱，常用的有：

* `sort`：排序；
* `binarySearch`：查找；
* `shuffle`：打乱；
* `unmodifiableList` 等：返回**不可变**版本的列表（你改会直接抛异常）。

```java
List<Integer> a = new ArrayList<>(Arrays.asList(3,1,2));
Collections.sort(a);
Collections.shuffle(a);

List<Integer> ro = Collections.unmodifiableList(a);
// ro.add(7); // UnsupportedOperationException
```

---

## 异常处理

先从一个真实问题开始：程序出错了，你打算怎么把错误传出去？

比如你写了个“读文件并返回第一行”的函数：

* 如果成功，返回字符串；
* 如果失败（文件不存在/权限不够），你打算返回什么？空字符串？null？-1？

你会发现“用返回值表达错误”很别扭：调用者还得写一堆 if 去判别，而且很容易漏掉。

异常这章的核心思想就是，程序出问题了，不要靠 `if (error) return;` 这种“自闭式处理”，而是用异常把错误信息传出去，让调用者决定怎么处理。

### 异常与异常类

**异常 (Exception)** 在 Java 里是一套类体系，根在 `Throwable`：

* `Error`：系统级问题，一般你处理不了也不该处理；
* `RuntimeException`：运行时异常（非检查异常），编译器不逼你处理；
* `Exception`：程序可处理的异常，一般编译器会要求你检查。

另一个常考点是“检查异常”和“非检查异常”：

* **非检查异常 (Unchecked Exception)**：基本都是 `RuntimeException` 的子类，比如 `NullPointerException`、`ArrayIndexOutOfBoundsException`；
* **检查异常 (Checked Exception)**：比如 `IOException`，编译器会强制你 `try-catch` 或 `throws`；
* 所以你看到方法签名里写了 `throws IOException`，这就是在提醒你“记得处理异常”。

这里我直接把 PPT 的图摘下来用了，表示异常类的层级结构。

![image-20260108152201115](./assets/image-20260108152201115.png)

![image-20260108152228233](./assets/image-20260108152228233.png)

### *捕获异常

最常见结构就是 `try-catch-finally`：

```java
try {
    int x = 10 / 0;
    System.out.println(x);
} catch (ArithmeticException e) {
    System.out.println("除以 0 了：" + e.getMessage());
} finally {
    System.out.println("finally 总会执行，无论有没有异常");
}
```

finally 块是给你做资源释放、状态恢复的，不要把业务逻辑塞进去。

补充一点，finally 通常会执行，但如果你在 try 里 `System.exit(0)`（立刻结束进程）了那就不会再执行。

#### 异常对象常用方法

你至少认识这几个，方便弄清楚异常的信息

* `getMessage()`：异常信息；
* `toString()`：异常类型 + 信息；
* `printStackTrace()`：打印调用栈（常用来调试）。

### 捕获多个异常

Java 7 之后支持多异常捕获：

```java
try {
    // ...
} catch (IOException | SQLException e) {
    e.printStackTrace();
} catch (Exception e) {
  	// 可能是意料之外的情况，考虑重新抛出去
  	throw e;
}
```

注意：

* `|` 连接的异常之间**不能有**父子关系；
* 多 catch 的顺序要从**“子类到父类”**，否则编译器会警告你，后面的 catch 块捕获不到异常。

### *throws 和 throw

不要弄混这两个关键字！前者用于方法签名，后者用于抛出异常对象：

* `throws` 写在方法签名上，表示“我不处理，往上抛”——传递抛出的异常；
* `throw` 写在方法体里，表示“我现在就抛一个异常”——我这里要产生抛出的异常。

```java
static int parsePositive(String s) throws NumberFormatException {
    int n = Integer.parseInt(s);
    if (n <= 0) {
        throw new IllegalArgumentException("n must be positive");
      	// 这里没有使用 try 块处理抛出的异常，所以方法签名上用 throws 来传递
      	// 等待该方法的调用者处理（或接着往上传递）异常
    }
    return n;
}
```

### try-with-resources

只要资源实现了 `AutoCloseable`，你就可以用 **try-with-resources (TWR)** 自动关资源，常见于 [I/O 操作](##输入输出)：

```java
try (FileInputStream in = new FileInputStream("a.bin")) {
    // 读文件
} // 自动 close()
```

它最大的价值是避免了你在 finally 里写一大堆冗长的资源释放代码。

### 自定义异常类

自定义异常一般两种思路：

* 继承 `Exception`：做检查异常，让调用者必须处理；
* 继承 `RuntimeException`：做运行时异常，调用者可选处理。

一个最小自定义异常：

```java
class MismatchException extends Exception {
    public MismatchException(String msg) {
        super(msg);
    }
}
```

### 断言

**断言 (Assertion)** 用于“我认为这里必然成立”的检查，应该不怎么会考：

```java
int x = 10;
assert x > 0 : "x 应该是正数"; // 不满足就会直接抛出*AssertionError*！
```

默认情况下断言是关闭的，需要在运行时用 JVM 选项 `-ea` 打开。**一般不用于可恢复的业务校验。**

---

## *输入输出

这一章的内容很多而且过于乱七八糟，如果你需要了解细节，我建议你自己去翻一下 PPT。笔者在这一章主要介绍考试需要你写 I/O 的时候怎么写，给出几种公式写法。**用的时候记得写对应 import 语句！**

### 向文件写入文本数据

一种最简单的写法如下，综合了几个知识点：

* 使用 TWR 语句**自动释放**资源；
* 使用 `FileWriter` 读写文件，它在文件不存在时会**自动创建**，已存在时**默认会自动清空**；
    * 除非你调用构造函数时将 `append` 为真，如： `new FileWriter("out.txt", true)` 
    
* 使用 `write()` 方法写入文本，注意它不会和 `println()` 一样自动换行！

```java
import java.io.FileWriter;
import java.io.Writer;
import java.io.IOException;
// 实在不记得了你就偷懒写个 import java.io.* 吧

public class Program {
    public static void main(String[] args) throws IOException {
        try (Writer w = new FileWriter("out.txt")) {
            w.write("Hello\n");
            w.write("World\n");
        }
    }
}

```

### 从文件读取文本数据

注意这里的 `FileReader` 需要保证文件存在才能用，否则会抛出异常。我们使用带有缓冲的 `BufferedReader` 来实现读取文本行（有缓冲速度更快），这是教材推荐的做法：

```java
import java.io.FileReader;
import java.io.BufferedReader;
import java.io.Reader;
import java.io.IOException;

public class Program {
    public static void main(String[] args) throws IOException {
        try (BufferedReader r = new BufferedReader(new FileReader("out.txt"))) {
            String line;
            while ((line = r.readLine()) != null) {
                System.out.println(line);
            }
        }
      	// 这里没写 catch 语句，所以 main 函数要用 throws 标记可能抛出异常
    }
}

```

### 对文件进行二进制的读写

另外作业里涉及过一个二进制的读写，所以这里贴一个 `DataStream` 的：

```java
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class Program {
    public static void main(String[] args) throws IOException {
        // 二进制形式写入整数
        try (DataOutputStream out =
                 new DataOutputStream(new FileOutputStream("data.dat"))) {
            for (int i = 1; i <= 5; i++) {
                out.writeInt(i * 10);
            }
        }

        // 二进制形式读取整数
        try (DataInputStream in =
                 new DataInputStream(new FileInputStream("data.dat"))) {
            for (int i = 0; i < 5; i++) {
                int x = in.readInt();
                System.out.println(x);
            }
        }
    }
}

```

### *用 Scanner 从文件读数据

这种情况也很常见，类似 C++ 考试需要你从一个输入文件里读数据：

```java
import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

public class Program {
    public static void main(String[] args) throws FileNotFoundException {
        Scanner sc = new Scanner(new File("nums.txt")); // 我们直接用 Scanner

        int sum = 0;
        while (sc.hasNextInt()) {
            int x = sc.nextInt(); // 读取下一个以空格为分隔符的整数
            sum += x;
        }

        sc.close();
        System.out.println("sum = " + sum);
    }
}
```

---

## 多线程基础

这一章和上一章一样有太多细枝末节的东西，而且本来就是很难的话题。笔者认为考试如果真考到那种程度应该没多少人弄得明白，所以也仅作简要介绍并给出几个公式写法。

**线程 (Thread)** 可以理解成“程序执行的最小单位”。多线程就是多条执行路径同时跑。

### 创建任务和线程

创建线程常见两条路：

* 实现 `Runnable`：推荐，类还能继承别的父类；
* 继承 `Thread`：写起来更直观，但会**占掉你唯一的继承位置**。

#### Runnable

```java
class MyTask implements Runnable {
    @Override
    public void run() {
        System.out.println("task running");
    }
}


// 在别的函数里
Thread t = new Thread(new MyTask());
t.start(); // 注意：是 start 不是 run
```

#### Thread

```java
class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("thread running");
    }
}

// 在别的函数里
new MyThread().start();
```

`start()` 才会真正启动新线程，**直接调用 `run()` 只是普通方法调用**。

### volatile 关键字

`volatile` 是一个**轻量级的并发关键字**，它解决的不是“原子性”，而是**可见性问题**。当一个线程修改了内存中某个地方的数据时，对应的寄存器可能不会同时改变，另一个线程可能将寄存器里的旧值当作当前的值。这个关键字就是用来解决这个问题的。

```java
class Worker implements Runnable {
    private volatile boolean running = true;

    @Override
    public void run() {
        while (running) {
            // do work
        }
        System.out.println("worker stopped");
    }

    public void stop() {
        running = false;
    }
}

// 在某个 main 函数中
Worker w = new Worker();
Thread t = new Thread(w);
t.start();
w.stop(); // 在另一个线程中
```

如果没有 `volatile`，`run()` 里的 `while (running)`  可能永远读到的是旧值 `true`，线程可能**无法停止**；加上 `volatile` 之后，每次读取 `running` 都从主内存取，一个线程的修改，其他线程**立刻可见**。

### *synchronized 关键字

我们用一个作业里出现过的银行账户存取的例子来说明它的作用，先看这个版本：

```java
class Account {
    private int balance = 10000;
    // 存款方法-没加synchronized
    public void deposit(int amount) { balance += amount; }
    // 取款方法-没加synchronized
    public void withdraw(int amount) { balance -= amount; }
    public int getBalance() { return balance; }
}

class Program {
    public static void main(String[] args) {
        Account account = new Account(); // 初始值：10000
        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                account.deposit(5);
            }
        }); // 创建线程 t1 进行 1000 次取钱 5 操作

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                account.withdraw(5);
            }
        }); // 创建线程 t2 进行 1000 次存入 5 操作

        t1.start();
        t2.start(); // 启动两个线程

        try {
            t1.join();
            t2.join(); // 等待两个线程执行完
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
				// 猜一猜这里最终的余额是多少呢？
        System.out.println("Final balance: " + account.getBalance());
    }
}
```

结果可能出乎你的预料（可能是包括 10000 在内的很多值），而且每次结果都不一样：

```plain
Final balance: 9730
```

究其根本，在于 `balance++` 和 `balance--` **不是原子操作**，它们其实相当于三个步骤：

```java
int t = balance; // 读取
t = t + 5;       // 增减
balance = t;     // 写回
```

所以我们设想的它们的执行顺序可能是这样的，这样最终的 balance 不变：

```java
int t1 = balance; // 读取
t1 = t1 + 5;      // 增5
balance = t1;     // 写回
int t2 = balance; // 再读取
t2 = t2 - 5;      // 减5
balance = t2;     // 再写回
```

但实际上在多线程的情况下你不能保证上面那六个步骤是按顺序执行的，于是可能出现：

```java
int t1 = balance; // 读取
int t2 = balance; // 再读取
t1 = t1 + 5;      // 增5
t2 = t2 - 5;      // 减5
balance = t1;     // 写回
balance = t2;     // 再写回，这时 balance 事实上变成了 balance - 5
```

`synchronized` 在这里起到的作用是，调用这两个方法时**锁住**当前的 `Account` 对象，阻止另一个线程修改它，从而保证了不会有多个线程同时在读/写 `balance` 字段，进而保证它的线程安全。

```java
class Account {
    private int balance = 10000;
    // 存款方法-加synchronized
    public synchronized void deposit(int amount) { balance += amount; }
    // 取款方法-加synchronized
    public synchronized void withdraw(int amount) { balance -= amount; }
    public int getBalance() { return balance; }
}

class Program {
    public static void main(String[] args) {
        Account account = new Account(); // 初始值：10000
        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                account.deposit(5);
            }
        }); // 创建线程 t1 进行 1000 次取钱 5 操作

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                account.withdraw(5);
            }
        }); // 创建线程 t2 进行 1000 次存入 5 操作

        t1.start();
        t2.start(); // 启动两个线程

        try {
            t1.join();
            t2.join(); // 等待两个线程执行完
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
				// 稳定的 10000
        System.out.println("Final balance: " + account.getBalance());
    }
}
```

改造后的输出总是 10000。

### 并发工具

一些原子操作工具类似上面 `synchronized` 功能，了解即可。其他的大胆推测不考。

### 真题：改造单线程写法

这里摘一个 21-22 真题的例子，演示如何把一个单线程写法改成多线程的。

原始代码：

```java
class RealTimeCorrectionModel extends CommonModel {
    private int result;

    public int getResult() {
        return result;
    }

    // 单线程计算方法
    public void calculate() {
        // 长时间的计算过程
      	// ...
        result = (int)(Math.random() * 1000); // 仅作示意
    }
}

public class MainApp {
    public static void main(String[] args) {
        var theModels = new RealTimeCorrectionModel[3];

        // 顺序执行 3 次计算（单线程）
        for (int i = 0; i < 3; i++) {
            // ... 此处填充程序，基于 RealTimeCorrectionModel 类启动 3 个线程并行计算
        }
				// ... 此处存在耗时*足够长*的操作可确保所有运算已经完成
      
        // 输出结果
        for (int i = 0; i < 3; i++) {
            System.out.println(
                "Result " + i + ": " + theModels[i].getResult()
            );
        }
    }
}

```

要求对程序进行必要的改造和填充，实现多线程并行计算（忽略 import 和异常处理）。

1. 改造 RealTimeCorrectionModel 类，使其支持线程化，并对其 calculate()方法做**同步化改造**，即杜绝一个RealTimeCorrectionModel 对象被两个线程同时调用 calculate()方法；

2. 填充 main() 方法 for 循环中的程序，基于 RealTimeCorrectionModel 类启动 3 个线程，并启动计算；

改法：

1. 由于原类定义时已经继承了一个类，所以我们没法让它继承自 `Thread`，而是**为它实现 `Runnable` 接口**；
2. 用 `synchronized` 关键字禁止并发进入同一个对象的 `calculate()` 方法；
3. 创建线程并调用 `start()` 方法。

```java
class RealTimeCorrectionModel extends CommonModel implements Runnable {
    private int result;

    public int getResult() {
        return result;
    }

    // 同一个对象的 calculate 不允许并发进入
    public synchronized void calculate() {
        // 长时间计算过程（示意）
        for (int i = 0; i < 1_000_000; i++) {
            // do something...
        }
        result = (int)(Math.random() * 1000); // 示例：给个结果
    }

    @Override
    public void run() { // 实现 Runnable 接口
        calculate();
    }
}

public class MainApp {
    public static void main(String[] args) {
        var theModels = new RealTimeCorrectionModel[3];
        Thread[] threads = new Thread[3];

        // 启动 3 个线程并行计算
        for (int i = 0; i < 3; i++) {
            theModels[i] = new RealTimeCorrectionModel();
            threads[i] = new Thread(theModels[i]); // 创建线程
            threads[i].start(); // 调用 start() 方法
        }
      
				// ... 此处存在耗时*足够长*的操作可确保所有运算已经完成


        // 输出结果
        for (int i = 0; i < 3; i++) {
            System.out.println("Result " + i + ": " + theModels[i].getResult());
        }
    }
}
```

注意上面的第 1 行，第 9 行，第 17-20 行，第 26-33 行是添加/修改的内容。

## 写在最后

恭喜你！到此你就已经面向考试地速通完了整个《面向对象程序设计》的课程！

如果想要检查自己的速通效果，你可以回到手册的目录部分，尝试自己根据[目录](##目录)的层次回忆各个章节的内容，尤其是带有 * 标注的章节（这是笔者推测考试可能重点考察的内容），独立地**输出**一份思维导图或者笔记。如果能够做到，说明你至少已经吸收了本手册的大部分内容。

预祝你考试顺利！

2026 年 1 月 8 日，Rhapsody0x1 from *WHU OSI & LUG*，于珞珈山下。
