---
layout: post
title: Java基础
categories: train
tags: [java,javase,train]
avatarimg: "/img/head.jpg"
author: 王一帆
published: false

---

一种记录、分析、总结、组织、讨论及解释信息的、有插图或无插图的、硬抄或平装的、加套或不加套的，包含有前言、介绍、目录表、索引的用以增长知识、加深理解、提升并教育人类大脑的装置，该装置需要视觉、有触碰的感官形式使用。 --- 《三傻大闹宝莱坞》

# 抽象

上面的长篇大论实际上是要解释什么是"书"！在电影中是男主角用来讽刺教授的死板的。实际上，仔细想来，我们所熟悉的任何东西都需要长篇大论来解释，而有些词甚至难以解释！比如说：

- 人
- 1
- 美：难以解释，很感性的东西

我们不断的学习知识来认知这个世界。而我们所学习的知识实际上是对这个世界的抽象！

和自然语言一样，编程语言也是对现实世界的抽象。自然语言是抽象给我们来认知，而编程语言则是抽象给计算机来认知。计算机擅长高频次简单计算，所以针对计算机的抽象与数学更接近。

所以我们从多项式开始，来引出编程语言！首先看如下一组多项式：

{% endhighlight %}
1
1 + 1
(1 + 1) * 3 + (1 + 1)
(1 + 1) * 3 + (1 + 2)
{% endhighlight %}

- 请找出如上多项式的共同点？
- 数学上如何处理上面的问题呢？

{% endhighlight %}
令 a = 1 + 1,b = 1 + 2
1       =>    (1 + 1) - 1  =>  a - 1  => 1 * a + (-1)
1 + 1   =>    a   =>  1 * a + (0 * a)
(1 + 1) * 3 + (1 + 1)    =>   3 * a + a
(1 + 1) * 3 + (1 + 2)    =>   3 * a + b
{% endhighlight %}

可以看出:上面的多项式都可以表示为na+b的形式

那编程语言如何处理这些问题呢?

{% highlight java %}
int a = 1 + 1;
int b = 1 + 2;
//a,b称为变量，类似自然语言中的代词。变量是编程语言中最基本的抽象
//1称为字面量
1       =>    (1 + 1) - 1  =>  a - 1  => 1 * a + (-1)
1 + 1   =>    a   =>  1 * a + (0 * a)
(1 + 1) * 3 + (1 + 1)    =>   3 * a + a
(1 + 1) * 3 + (1 + 2)    =>   3 * a + b
{% endhighlight %}

可以看出，基本是没有区别的!

在我们动手运行代码前！我们先来搭建Java环境!


# Java环境搭建

-   到[Oracle官方网站](http://www.oracle.com/technetwork/java/javase/downloads/index.html)下载相应操作系统的JDK进行安装
-   打开命令行,输入

{% endhighlight %}
java -version
{% endhighlight %}

得到如下信息，则表示搭建成功!

{% highlight sh %}
java version "1.8.0"
Java(TM) SE Runtime Environment (build 1.8.0-b132)
Java HotSpot(TM) 64-Bit Server VM (build 25.0-b70, mixed mode)
{% endhighlight %}

Java中需要理解两个路径：

-   path是系统查找程序的路径
-   classpath是Java查找类的路径

## 练习

-   请从服务器下载jdk,并进行安装
-   验证安装是否成功
-   修改path,理解path的作用

## 编辑器选择

-   文本编辑器
-   Notepad++
-   Editplus
-   UltraEdit
-   TextMate
-   Sublime Text
-   Vim
-   Emacs

## 学习曲线

![]({{site.CDN_PATH}}../assets/javabasic/5.jpg)

## 练习

-   下载Notepad++,并打开

# IDE选择

-   eclipse
-   Netbeans
-   Intellij IDEA

# 第一个程序

-   创建文件Hello.java
-   输入如下代码

{% endhighlight %} {.java}

//例子代码
public class Hello{
    public static void main(String[] args){
        int a = 1 + 1;
        int b = 1 + 2;
        System.out.println("(1 + 1) * 3 + (1 + 2) = " + (a * 3 + b));
    }
}
{% endhighlight %}

-   打开命令行,切换到文件所在路径
-   输入javac Hello.java
-   输入java Hello

## 课堂练习


-   请按步骤,新建Hello.java程序,并运行

# 运行机制

--------------

![]({{site.CDN_PATH}}/home/ivan/my/teach/java/javase/01_basic/file/2.jpg)

-   思考,Java如何做到跨平台?

##

![]({{site.CDN_PATH}}/home/ivan/my/teach/java/javase/01_basic/file/1.jpg)

# 代码组成

----------------------------

{% endhighlight %} {.java}

//例子代码
public class Hello{
    public static void main(String[] args){
        int a = 1 + 1;
        int b = 1 + 2;
        System.out.println("(1 + 1) * 3 + (1 + 2) = " + (a * 3 + b));
    }
}
{% endhighlight %}

-   注释
-   标识符(Hello,args)
-   关键字(public,class,static,void)
-   字面量(字符串) ("(1 + 1) * 3 + (1 + 2) = ")
-   变量(args,a,b)
-   类
-   方法

## 注释


{% endhighlight %} {.java}

//单行注释
{% endhighlight %}

{% endhighlight %} {.java}

/*
多行注释
*/
{% endhighlight %}

{% endhighlight %} {.java}

/**
 * javadoc注释
 */
{% endhighlight %}

## 标识符

-   标识符由字母（A-Z或者a-z）,美元符（\$）、下划线（\_）或数字组成
-   标识符都应该以字母（A-Z或者a-z）,美元符（\$）、或者下划线（\_）开始
-   关键字不能用作标识符
-   标识符是大小写敏感的
-   合法标识符举例：age、\$salary、~value~、\_~1value~
-   非法标识符举例：123abc、-salary

## 关键字


![]({{site.CDN_PATH}}/home/ivan/my/teach/java/javase/01_basic/file/3.jpg)

## 字面量


-   字符串:"Hello"
-   字符:'H','e','l','l','o'
-   布尔值:true,false
-   数值:1,2.4

## 变量


-   变量就是一个命名的内存块
-   变量只能存储一种具体类型的数据
-   使用变量之前必须声明(变量名和类型)
-   使用前需要初始化

-   局部变量:方法或语句块内部
-   成员变量:方法外,类内部

## 类和对象


-   类是对一个特定类型对象的描述,它定义了一种新的类型
-   类定义中的成员变量可以是任意类型
-   对象是类的实体,使用关键字new来创建

## 方法


-   实例方法
-   静态方法(static)
-   方法调用[递归调用]

{% endhighlight %} {.java}

public class Hello{
    public static void main(String[] args){
        System.out.println("Hello");
        Hello.main(null);
    }
}
{% endhighlight %}

# 基本类型

--------------

![]({{site.CDN_PATH}}/home/ivan/my/teach/java/javase/01_basic/file/4.jpg)

## 使用递归来计算基本类型的范围

{% endhighlight %} {.java}

public class Test{
    public static long p(int num){
        if(num == 0) return 1;
        return 2 * p(--num);  //--num 与 num--
    }

    public static long caculateRange(int num){
        if(num == 0) return 1;
        return p(num) + caculateRange(--num);
    }
}
{% endhighlight %}

# 数组

----------------------------

数组是相同类型的,用一个标示符名称封装到一起的一个对象序列或基本类型数据序列

## 一维数组

{% endhighlight %} {.java}

int[] arr;
int arr[];
//初始化
arr = {1,2,3};
arr = new int[3];  //默认初始化
//使用,下标从0开始
arr[0];
{% endhighlight %}

## 多维数组

{% endhighlight %} {.java}

int[][] arr = { {1,2,3},{4,5,6}};
int[][][] arr = new int[2][3][4];
//每一维长度可以不同
{% endhighlight %}

# 运算符(操作符)

--------------

**运算符指明对操作数的运算方式**

-   算术运算符
-   关系运算符
-   位运算符
-   逻辑运算符
-   赋值运算符
-   其他运算符

## 算术运算符

{% highlight sh %}

+    加法 - 相加运算符两侧的值
-    减法 - 左操作数减去右操作数
*    乘法 - 相乘操作符两侧的值
/    除法 - 左操作数除以右操作数
%    取模 - 右操作数除左操作数的余数
++   自增 - 操作数的值增加1
-    自减 - 操作数的值减少1
{% endhighlight %}

## 实例

{% endhighlight %} {.java}

public class Test {
  public static void main(String args[]) {
     int a = 10;
     int b = 20;
     int c = 25;
     int d = 25;
     System.out.println("a + b = " + (a + b) );
     System.out.println("a - b = " + (a - b) );
     System.out.println("a * b = " + (a * b) );
     System.out.println("b / a = " + (b / a) );
     System.out.println("b % a = " + (b % a) );
     System.out.println("c % a = " + (c % a) );
     System.out.println("a++   = " +  (a++) );
     System.out.println("b--   = " +  (a--) );
     System.out.println("d++   = " +  (d++) );
     System.out.println("++d   = " +  (++d) );
  }
}
{% endhighlight %}

## 关系运算符

{% highlight sh %}

==   检查如果两个操作数的值是否相等，如果相等则条件为真
=!   检查如果两个操作数的值是否相等，如果值不相等则条件为真
>    检查左操作数的值是否大于右操作数的值，如果是那么条件为真
<    检查左操作数的值是否小于右操作数的值，如果是那么条件为真
>=   检查左操作数的值是否大于或等于右操作数的值，如果是那么条件为真
<=   检查左操作数的值是否小于或等于右操作数的值，如果是那么条件为真
{% endhighlight %}

## 实例

{% endhighlight %} {.java}

public class Test {
  public static void main(String args[]) {
     int a = 10;
     int b = 20;
     System.out.println("a == b = " + (a == b) );
     System.out.println("a != b = " + (a != b) );
     System.out.println("a > b = " + (a > b) );
     System.out.println("a < b = " + (a < b) );
     System.out.println("b >= a = " + (b >= a) );
     System.out.println("b <= a = " + (b <= a) );
  }
}
{% endhighlight %}

## 位运算符

{% highlight sh %}

&    按位与操作符，当且仅当两个操作数的某一位都非0时候结果的该位才为1
|    按位或操作符，只要两个操作数的某一位有一个非0时候结果的该位就为1
^    按位异或操作符，两个操作数的某一位不相同时候结果的该位就为1
~    按位补运算符翻转操作数的每一位
<<   按位左移运算符。左操作数按位左移右操作数指定的位数
>>   按位右移运算符。左操作数按位右移右操作数指定的位数
>>>      按位右移补零操作符。左操作数的值按右操作数指定的位数右移，移动得到的空位以零填充。
{% endhighlight %}

## 实例

{% endhighlight %} {.java}

public class Test {
  public static void main(String args[]) {
     int a = 60; /* 60 = 0011 1100 */
     int b = 13; /* 13 = 0000 1101 */
     int c = 0;
     c = a & b;       /* 12 = 0000 1100 */
     System.out.println("a & b = " + c );
     c = a | b;       /* 61 = 0011 1101 */
     System.out.println("a | b = " + c );
     c = a ^ b;       /* 49 = 0011 0001 */
     System.out.println("a ^ b = " + c );
     c = ~a;          /*-61 = 1100 0011 */
     System.out.println("~a = " + c );
     c = a << 2;     /* 240 = 1111 0000 */
     System.out.println("a << 2 = " + c );
     c = a >> 2;     /* 215 = 1111 */
     System.out.println("a >> 2  = " + c );
     c = a >>> 2;     /* 215 = 0000 1111 */
     System.out.println("a >>> 2 = " + c );
  }
}
{% endhighlight %}

## 逻辑运算符

{% highlight sh %}

&&   称为逻辑与运算符。当且仅当两个操作数都为真，条件才为真
||   称为逻辑或操作符。如果任何两个操作数任何一个为真，条件为真
!    称为逻辑非运算符。用来反转操作数的逻辑状态。如果条件为true，则逻辑非运算符将得到false。
{% endhighlight %}

## 实例

{% endhighlight %} {.java}

public class Test {
  public static void main(String args[]) {
     boolean a = true;
     boolean b = false;
     System.out.println("a && b = " + (a&&b));
     System.out.println("a || b = " + (a||b) );
     System.out.println("!(a && b) = " + !(a && b));
  }
}
{% endhighlight %}

## 赋值运算符

{% highlight sh %}

=    简单的赋值运算符，将右操作数的值赋给左侧操作数
+=   加和赋值操作符，它把左操作数和右操作数相加赋值给左操作数
-=   减和赋值操作符，它把左操作数和右操作数相减赋值给左操作数
*=   乘和赋值操作符，它把左操作数和右操作数相乘赋值给左操作数
/=   除和赋值操作符，它把左操作数和右操作数相除赋值给左操作数
%=   取模和赋值操作符，它把左操作数和右操作数取模后赋值给左操作数
<<=  左移位赋值运算符
>>=  右移位赋值运算符
&=   按位与赋值运算符
^=   按位异或赋值操作符
|=   按位或赋值操作符
{% endhighlight %}

## 实例

{% endhighlight %} {.java}

public class Test {
  public static void main(String args[]) {
     int a = 10; int b = 20; int c = 0; c = a + b;
     System.out.println("c = a + b = " + c );
     c += a ;
     System.out.println("c += a  = " + c );
     c -= a ;
     System.out.println("c -= a = " + c );
     c *= a ;
     System.out.println("c *= a = " + c );
     a = 10; c = 15; c /= a ;
  }
}
{% endhighlight %}

## 三目运算符

{% highlight sh %}

variable x = (expression) ? value if true : value if false
{% endhighlight %}

{% endhighlight %} {.java}

public class Test {
   public static void main(String args[]){
        int a , b;
        a = 10;
        b = (a == 1) ? 20: 30;
        System.out.println( "Value of b is : " +  b );
        b = (a == 10) ? 20: 30;
        System.out.println( "Value of b is : " + b );
   }
}
{% endhighlight %}

## instanceof

{% highlight sh %}

( Object reference variable ) instanceof  (class/interface type)
{% endhighlight %}

{% endhighlight %} {.java}

"James" instanceof String;
{% endhighlight %}

## 运算符优先级

{% highlight sh %}

后缀     () [] . (点操作符)
一元   ++ - ！ ~
乘性   * / %
加性   + -
移位   >> >>>  <<
关系   >> = << =
相等   ==  !=
按位与  &
按位异或     ^
按位或  |
逻辑与  &&
逻辑或  ||
条件   ? ：
赋值   = += -= *= /= %= >>= <<= ＆= ^= |=
逗号   ,
{% endhighlight %}

# 控制执行流程

----------------------------

- if-else
- switch
- while
- do-while
- for
- foreach
- break
- continue
- return


## if

{% endhighlight %} {.java}

if(布尔表达式) {
   //如果布尔表达式为true将执行的语句
}
{% endhighlight %}

{% endhighlight %} {.java}

public class Test {
   public static void main(String args[]){
      int x = 10;
      if( x < 20 ){
         System.out.print("这是 if 语句");
      }
   }
}
{% endhighlight %}

## if...else

{% endhighlight %} {.java}

if(布尔表达式){
   //如果布尔表达式的值为true
}else{
   //如果布尔表达式的值为false
}
{% endhighlight %}

{% endhighlight %} {.java}

public class Test {
   public static void main(String args[]){
      int x = 30;
      if( x < 20 ){
         System.out.print("这是 if 语句");
      }else{
         System.out.print("这是 else 语句");
      }
   }
}
{% endhighlight %}

## if...else if...else


{% endhighlight %} {.java}

if(布尔表达式 1){
   //如果布尔表达式 1的值为true执行代码
}else if(布尔表达式 2){
   //如果布尔表达式 2的值为true执行代码
}else if(布尔表达式 3){
   //如果布尔表达式 3的值为true执行代码
}else {
   //如果以上布尔表达式都不为true执行代码
}
{% endhighlight %}

## 实例

{% endhighlight %} {.java}

public class Test {
   public static void main(String args[]){
      int x = 30;
      if( x == 10 ){
         System.out.print("Value of X is 10");
      }else if( x == 20 ){
         System.out.print("Value of X is 20");
      }else if( x == 30 ){
         System.out.print("Value of X is 30");
      }else{
         System.out.print("This is else statement");
      }
   }
}
{% endhighlight %}

## if嵌套

{% endhighlight %} {.java}

public class Test {
   public static void main(String args[]){
      int x = 30;
      int y = 10;
      if( x == 30 ){
         if( y == 10 ){
             System.out.print("X = 30 and Y = 10");
          }
       }
    }
}
{% endhighlight %}

## switch

{% endhighlight %} {.java}

switch(expression){
    case value :
       //语句
       break; //可选
    case value :
       //语句
       break; //可选
    //你可以有任意数量的case语句
    default : //可选
       //语句
}
{% endhighlight %}

## 注意点

-   switch语句中的变量类型只能为byte、short、int或者char,enum.
    **JDK7开始支持String**
-   switch语句可以拥有多个case语句。每个case后面跟一个要比较的值和冒号。
-   case语句中的值的数据类型必须与变量的数据类型相同，而且只能是常量或者字面常量。
-   当变量的值与case语句的值相等时，那么case语句之后的语句开始执行，直到break语句出现才会跳出switch语句。
-   当遇到break语句时，switch语句终止。程序跳转到switch语句后面的语句执行。
-   switch语句可以包含一个default分支，该分支必须是switch语句的最后一个分支。default在没有case语句的值和变量值相等的时候执行。

## 实例

{% endhighlight %} {.java}

public class Test {
   public static void main(String args[]){
      char grade = 'C';
      switch(grade) {
         case 'A' :
            System.out.println("Excellent!");
            break;
         case 'B' :
         case 'C' :
            System.out.println("Well done");
            break;
         case 'D' :
            System.out.println("You passed");
         case 'F' :
            System.out.println("Better try again");
            break;
         default :
            System.out.println("Invalid grade");
      }
      System.out.println("Your grade is " + grade);
   }
}
{% endhighlight %}

## while

{% endhighlight %} {.java}

while( 布尔表达式 ) {
    //循环内容
}
{% endhighlight %}

{% endhighlight %} {.java}

public class Test {
   public static void main(String args[]) {
      int x = 10;
      while( x < 20 ) {
         System.out.print("value of x : " + x );
         x++;
         System.out.print("\n");
      }
   }
}
{% endhighlight %}

## do-while

{% endhighlight %} {.java}

do {
       //代码语句
}while(布尔表达式);
{% endhighlight %}

{% endhighlight %} {.java}

public class Test {
   public static void main(String args[]){
      int x = 10;
      do{
         System.out.print("value of x : " + x );
         x++;
         System.out.print("\n");
      }while( x < 20 );
   }
}
{% endhighlight %}

## for

{% endhighlight %} {.java}

for(初始化; 布尔表达式; 更新) {
    //代码语句
}
{% endhighlight %}

-   最先执行初始化步骤。可以声明并初始化一个或多个循环控制变量，也可以是空语句。
-   然后，检测布尔表达式的值。如果为true，循环体被执行。如果为false，循环终止，开始执行循环体后面的语句。
-   执行一次循环后，更新循环控制变量。
-   再次检测布尔表达式。循环执行上面的过程。

## 实例

{% endhighlight %} {.java}

public class Test {
   public static void main(String args[]) {
      for(int x = 10; x < 20; x = x+1) {
         System.out.print("value of x : " + x );
         System.out.print("\n");
      }
   }
}
{% endhighlight %}

## foreach

{% endhighlight %} {.java}

for(声明语句 : 表达式) {
   //代码句子
}
{% endhighlight %}

{% endhighlight %} {.java}

public class Test {
   public static void main(String args[]){
      int[] numbers = {10, 20, 30, 40, 50};
      for(int x : numbers){
         System.out.print( x );
         System.out.print(",");
      }
      System.out.print("\n");

      String[] names ={"James", "Larry", "Tom", "Lacy"};
      for(String name : names) {
         System.out.print( name );
         System.out.print(",");
      }
   }
}
{% endhighlight %}

## break

{% endhighlight %} {.java}

public class Test {
   public static void main(String args[]) {
      int [] numbers = {10, 20, 30, 40, 50};

      for(int x : numbers) {
         if(x == 30) {
          break;
         }
         System.out.print(x);
         System.out.print("\n");
      }
   }
}
{% endhighlight %}

## continue

{% endhighlight %} {.java}

public class Test {
   public static void main(String args[]) {
      int [] numbers = {10, 20, 30, 40, 50};
      for(int x : numbers) {
         if(x == 30) {
          continue;
         }
         System.out.print(x);
         System.out.print("\n");
      }
   }
}
{% endhighlight %}

## labeled break与labeled continue

{% endhighlight %} {.java}

public class Test {
   public static void main(String args[]) {
        int [] numbers = {10, 20, 30, 40, 50};
        outer: for(int x : numbers) {
            for(int i = 1;i < 3;i++) {
                if (x == 30) {
                    break outer;
                }
                System.out.print(x);
                System.out.print("\n");
            }
            System.out.println("outer" + x);
        }
    }
}
{% endhighlight %}

## return

-   跳出当前方法,并返回方法所需要的值(如果有返回值的话)

## 课堂练习

-   使用数组计算矩阵乘法

![]({{site.CDN_PATH}}/home/ivan/my/teach/java/javase/01_basic/file/6.jpg)

## 答案

{% endhighlight %} {.java}

public class Test{
    public static void main(String[] args){
        int[][] a = { {1,0,2},{-1,3,1}};
        int[][] b = { {3,1},{2,1},{1,0}};
        int[][] c = new int[2][2];
        for(int i = 0; i < 2;i++){
            int tmp = 0;
            int tmp2 = 0;
            for(int j = 0;j < 3;j++){
                tmp += a[i][j] * b[j][i];
                tmp2 += a[i][j] * b[j][1-i];
            }
            c[i][i] = tmp;
            c[i][1-i]=tmp2;
        }
        System.out.println(c[0][0]);
        System.out.println(c[0][1]);
        System.out.println(c[1][0]);
        System.out.println(c[1][1]);
    }
}
{% endhighlight %}

## 课后作业

- 理解path与classpath
- 请任意修改hello world,并尝试编译运行!如报错,请尝试理解报错信息!
- 请编写程序,打印如下图形.你能想出几种方法?你认为最简单的方法是哪种?

{% endhighlight %}

  1
 111
11111
{% endhighlight %}

- 请使用循环和递归分别实现斐波那契数列

{% endhighlight %}

1 1 2 3 5 8 13 ...
{% endhighlight %}

- 本机安装eclipse,并在eclipse中运行hello world
- 请到网上搜索面向对象编程,尝试理解面向对象编程,及其与面向过程编程的区别
