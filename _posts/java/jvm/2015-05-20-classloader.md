---
layout: post
title: ClassLoader
categories: jvm
tags: [classloader,tomcat,osgi]
avatarimg: "/img/head.jpg"
published: false

---


# 什么是ClassLoader?

Java程序都是由若干个.class文件组成，程序的运行过程即是对class文件的调用。
程序在启动的时候，并不会一次性加载程序所要用的所有class文件，而是根据程序的需要，
通过Java的类加载机制（ClassLoader）来动态加载某个class文件到内存当中的，
只有class文件被载入到了内存之后，才能被其它class所引用。
所以ClassLoader就是用来动态加载class文件到内存当中用的。

# Java默认提供的三个ClassLoader

- BootStrap ClassLoader：称为启动类加载器，是Java类加载层次中最顶层的类加载器，负责加载JDK中的核心类库，如：rt.jar、resources.jar、charsets.jar等
- Extension ClassLoader：称为扩展类加载器，负责加载Java的扩展类库，默认加载JAVA_HOME/jre/lib/ext/目下的所有jar。
- App ClassLoader：称为系统类加载器，负责加载应用程序classpath目录下的所有jar和class文件。

![](/assets/jvm/classloader/classloader.jpg)

示例:

```java
import com.sun.nio.zipfs.ZipFileStore;

public class Test {

    public static void main(String[] args) {
        System.out.println("1".getClass().getClassLoader());
        System.out.println(ZipFileStore.class.getClassLoader());
        System.out.println(A.class.getClassLoader());
    }
}

class A{}
```

输出结果:

```
null                          //BootStrap ClassLoader输出为null
sun.misc.Launcher$ExtClassLoader@3e10c986
sun.misc.Launcher$AppClassLoader@610f7612
```

<!-- more -->

# 双亲委托模型

ClassLoader使用的是双亲委托模型来搜索类的，每个ClassLoader实例都有一个父类加载器的引用（不是继承的关系，是一个包含的关系），
虚拟机内置的类加载器（Bootstrap ClassLoader）本身没有父类加载器，但可以用作其它ClassLoader实例的的父类加载器。

当一个ClassLoader实例需要加载某个类时，它在试图亲自搜索某个类之前，先把这个任务委托给它的父类加载器，这个过程是由上至下依次检查的：

- 首先由最顶层的类加载器Bootstrap ClassLoader试图加载，如果没加载到，
- 则把任务转交给Extension ClassLoader试图加载，如果也没加载到，
- 则转交给App ClassLoader 进行加载，如果它也没有加载得到的话，
- 则返回给委托的发起者，由它到指定的文件系统或网络等URL中加载该类。
- 如果它们都没有加载到这个类时，则抛出ClassNotFoundException异常。
- 否则将这个找到的类生成一个类的定义，并将它加载到内存当中，最后返回这个类在内存中的Class实例对象。

使用双亲委托模型主要是为了安全性.

如果不使用这种委托模式，那我们就可以随时使用自定义的Object来动态替代java核心api中定义的类型，
这样会存在非常大的安全隐患，而双亲委托的方式，就可以避免这种情况，因为Object已经在启动时就被引导类加载器（Bootstrcp ClassLoader）
加载，所以用户自定义的ClassLoader永远也无法加载一个自己写的Object，除非你改变JDK中ClassLoader搜索类的默认算法。

# class的唯一性判断

JVM在判定两个class是否相同时，不仅要判断两个类名是否相同，而且要判断是否由同一个类加载器实例加载的。
只有两者同时满足的情况下，JVM才认为这两个class是相同的。就算两个class是同一份class字节码，
如果被两个不同的ClassLoader实例所加载，JVM也会认为它们是两个不同class。
比如网络上的一个Java类org.classloader.simple.NetClassLoaderSimple，
javac编译之后生成字节码文件NetClassLoaderSimple.class，
ClassLoaderA和ClassLoaderB这两个类加载器并读取了NetClassLoaderSimple.class文件，
并分别定义出了java.lang.Class实例来表示这个类，对于JVM来说，它们是两个不同的实例对象，
但它们确实是同一份字节码文件，如果试图将这个Class实例生成具体的对象进行转换时，
就会抛运行时异常java.lang.ClassCaseException，提示这是两个不同的类型。

# 自定义ClassLoader

Java中提供的默认ClassLoader，只加载指定目录下的jar和class，如果我们想加载其它位置的类或jar时，
比如：要加载网络上的一个class文件，通过动态加载到内存之后，要调用这个类中的方法实现业务逻辑。
在这样的情况下，默认的ClassLoader就不能满足需求，所以需要定义自己的ClassLoader。

定义自已的类加载器：

- 继承java.lang.ClassLoader
- 重写父类的findClass方法
- 如没有特殊的要求，一般不建议重写loadClass搜索类的算法

# Tomcat ClassLoader

以Tomcat的ClassLoader为例，来说明自定义ClassLoader的应用

跟其他主流的Java Web服务器一样，Tomcat也拥有不同的自定义类加载器，达到对各种资源库的控制。一般来说，Java Web服务器需要解决以下四个问题：

- 同一个Web服务器里，各个Web项目之间各自使用的Java类库要互相隔离。
- 同一个Web服务器里，各个Web项目之间可以提供共享的Java类库。
- 服务器为了不受Web项目的影响，应该使服务器的类库与应用程序的类库互相独立。
- 对于支持JSP的Web服务器，应该支持热插拔（hotswap）功能。

对于以上几个问题，如果单独使用一个类加载器明显是达不到效果的，必须根据实际使用若干个自定义类加载器。

下面以本书主要剖析的Tomcat7为例，看看它的类加载器是怎样定义的？如图2-4-3，启动类加载器、扩展类加载器、应用程序类加载器这三个类加载器数据JDK级别的加载器，
他们是唯一的，我们一般不会对其做任何更改。接下来则是Tomcat的类加载器，在tomcat7中，最重要的一个类加载器是Common ClassLoader，
它的父类加载器是ApplicationClassLoader，负责加载 $CATALINA_BASE/lib、  $CATALINA_HOME/lib两个目录下所有的.class跟.jar文件。
而下面虚线框的两个类加载器有必要说明一下，如果在Tomcat5版本，这两个类加载器实例默认与Common ClassLoader实例不同，Common ClassLoader为他们的父类加载器。
而在Tomcat7中，这两个实例变量也存在，只是catalina.properties配置文件没有对server.loader跟share.loader两项进行配置，所以在程序里，
这两个类加载器实例就被赋值为CommonClassLoader实例，即一个tomcat实例其实就只有CommonClassLoader实例