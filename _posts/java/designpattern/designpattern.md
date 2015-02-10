拙劣设计
========

拙劣设计的症状
--------------

拙劣设计的症状如下：
-   僵化性(Rigidity):设计难以改变--很难对系统进行改动，因为每个改动都会迫使许多对系统其它部分的其他改动。
-   脆弱性(Fragility):设计易于遭到破坏--对系统的改动会导致系统中和改动的地方在概念上无关的许多地方出现问题。
-   牢固性(Immobility):设计难以重用--很难解开系统的纠结，使之成为一些可在其他系统中重用的组件。
-   粘滞性(Viscosity):难以做到正确的事情--做正确的事情比做错误的事情要困难。
-   不必要的复杂性(Needless
    Complexity):过分设计--设计中包含有不具任何直接好处的基础机构。
-   不必要的重复(Needless
    Repetition):滥用鼠标--设计中包含有重复的结构，而该重复的结构本可以使用单一的抽象进行统一。
-   晦涩性(Opacity):混乱得表达--很难阅读、理解。没有很好的表现出意图。

如何避免拙劣设计
----------------

1.  遵循敏捷实践去发现问题；
2.  应用设计原则去诊断问题；并且
3.  应用适当的设计模式去解决问题。

设计原则
========

总览
----

面向对象设计有如下几个原则
-   SRP 单一职责原则

就一个类而言，应该仅有一个引起它变化的原因。
-   OCP 开放--封闭原则

软件实体（类，模块，方法等）应该是可以扩展的，但是不可修改。
-   LSP Liskov替换原则

子类型必须能够替换掉它们的基类型
-   DIP 依赖倒置原则

抽象不应该依赖于细节。细节应该依赖于抽象。
-   ISP 接口隔离原则

不应该强迫客户依赖于它们不用的方法。接口属于客户，不属于它所在的类层次结构。
-   REP 重用发布等价原则

重用的粒度就是发布的粒度。
-   CCP 共同封闭原则

包中的所有类对于同一类性质的变化应该是共同封闭的。一个变化若对一个包产生影响，则将对该包中的所有类产生影响，而对其他的包不造成任何影响。
-   CRP 共同重用原则

一个包中的所有类应该是共同重用的。如果重用了包中的一个类，那么就要重用包中的所有类。
-   ADP 无环依赖原则

在包的依赖关系图中不允许存在环。
-   SDP 稳定依赖原则

朝着稳定的方向进行依赖。
-   SAP 稳定抽象原则

包的抽象程度应该和其稳定程度一致。 @\<br\>主要讨论前5个原则

SRP: The single responsibility principle 单一职责原则
-----------------------------------------------------

SRP看起来简单，单实际应用却很麻烦。因为如何确定职责是否单一没有一个统一的标准，主要还是看在系统中的实际情况。

OCP : Open-Close Principle开闭原则
----------------------------------

LSP: The Liskov substitution principle
--------------------------------------

DIP：依赖倒置原则
-----------------

ISP:接口隔离原则
----------------

基本原则
--------

-   封装变化Encapsulate what varies.
-   面向接口编程而非实现 Code to an interface rather than to an
    implementation.
-   优先使用组合而非继承 Favor Composition Over Inheritance
-   设计模式

单例模式(Singleton) :创建型模式:
--------------------------------

Singleton顾名思义就是只能创建一个实例对象。。所以不能拥有public的构造方法。。

``` {.java}
  public class Singleton{   
         private Singleton(){}   
  }  
```

既然构造方法是私有的，那么从外面不可能创建Singleton实例了。。只能从内部创建。。所以需要一个方法来创建此实例。。此方法肯定必须是static的。。

``` {.java}
  public class Singleton{      
         private Singleton(){}           
         public static Singleton getInstance(){   
              return [Singleton Instance];   
        }   
  }   
```

getInstance方法要返回一个Singleton实例。。就要一个Singleton类型的变量来存储。。声明一个Singleton类型的属性。。同样需要是static
的。。静态方法只能访问静态属性。。。

``` {.java}
  public class Singleton{      
         private Singleton(){}      

         public static Singleton getInstance(){   
              single=new Singleton();   
              return single;   
        }   
        private static Singleton single;   
  }   
```

如此就能获得Singleton的实例了。。但是并不能确保只生成一个实例。。。需做判断。。。

``` {.java}
public class Singleton{      
       private Singleton(){}      

       public static Singleton getInstance(){   
           if(single==null){   
                single=new Singleton();   
           }   
           return single;   
       }   
      private static Singleton single;   
}   
```

这样就可以了。。。
接着就牵扯到了线程问题。。。假设有两个线程。。thread1，thread2。。thread1运行到第5行，然后跳到了thread2。。也运行到第5行之后。。。此时两线程都得到single为空。。。那么就会有两个实例了。。。解决办法。。同步。。

``` {.java}
public class Singleton{         
       private Singleton(){}         

       public synchronized static Singleton getInstance(){      
           if(single==null){      
                single=new Singleton();      
           }      
           return single;      
       }      
      private static Singleton single;      
}      
```

还有一种方法，提前实例化。。。

``` {.java}
public class Singleton{         
       private Singleton(){}         

       public static Singleton getInstance(){      
                  return single;      
       }      
      private static Singleton single=new Singleton();      
}      
```

此时single的static修饰符有起到另一个作用。。。因为static类型的属性，只在类加载时初始化一次。。。以后不会再初始化了。。确保了只有一个实例。。。
最后一种方法是再head first design pattern上看到的。。double-checked
locking。。。

``` {.java}
public class Singleton{         
       private Singleton(){}         

       public static Singleton getInstance(){      
           if(single==null){      
               synchronized(Singleton.class){   
                  if(single==null){      
                     single=new Singleton();      
                  }   
               }   
           }      
           return single;      
       }      
      private volatile static Singleton single;      
}          
```

目前对Singleton的理解就这么多。

简单工厂(Simple Factory) :创建型模式:
-------------------------------------

简单工厂模式不在gof的23种设计模式之列，HeadFirst设计模式中也没把它列为设计模式，而只将其当做一种写法。简单工厂模式没辜负它的名字，真的非常简单，应该是设计模式里面最最简单的一个模式了。

它的作用就是将散落在各个地方的new方法，集合到一个类里面去，方便管理，它并不遵循设计原则。当需要增加一个类的时候，你还是需要修改工厂类，这违背了开闭原则。

具一个很简单的例子，假设有一个接口Pig，它下面有很多的实现，RedPig,GreenPig,BluePig.你调用他们的时候就需要new
RedPig(),newGreenPig(),new
BluePig()。而这些new散落在了各个客户类（就是调用Pig的那些类，Pig的子类就是服务类，不要将这里的客户和服务与web里面的混淆）里面，而简单工厂的作用就是将这些散落的类都集中到它内部管理。代码如下：

``` {.java}
public class PigFactory{
     public static Pig getPig(String type){
          if("red".equals(type)){
               return new RedPig();
          }else if("green".equals(type)){
               return new GreenPig();
          }else if("blue".equals(type)){
               return new BluePig();
          }
     }
}
```

这里提供了一个静态方法，根据传入的字符串来判断返回哪个具体的子类。此工厂又称为静态工厂方法。你也可以不用静态方法，这样每次取Pig都要先实例化工厂，这样一是资源的浪费，二是不符合逻辑啊，工厂只需要一个就够了。该怎么办呢？（看看单例模式）。当要添加子类类型的时候，是需要修改PigFactory类的，违背开闭原则，HeadFirst不将其称为设计模式也是有道理的。
对上面代码的改进就是使用反射，直接将子类的全路径传入，用Class.forName来实例化，这样就不需要修改工厂类了，但是每次都写类的全路径名，太累了。

``` {.java}
public class PigFactory{
     public static Pig getPig(String name){
          return Class.forName(name).newInstance();
     }
}
```

再进一步的改进就是使用配置文件，相当于给类的全路径名取个简称。这里就不多写了，看看spring吧！
简单工厂就说这么多，很简单。代码简单，目的性也很明确！

工厂方法(Factory Method) :创建型模式:
-------------------------------------

在简单工厂模式里面说到了其缺点，就是违背了开闭原则，后面也提到了解决办法。
不过这里有另一个解决办法，那就是工厂方法模式。
简单工厂模式的意图就是将分散在各处的new集中到一个地方，方便管理。而工厂方法模式是为了弥补简单工厂违背了开闭原则的缺点。
看下简单工厂模式代码，如果看过martin
fowler的重构这本书，应该大概知道怎么做。只要使用Replace Conditional with
Polymorphism（以多态取代条件式）即可

``` {.java}
public class PigFactory{
     public static Pig getPig(String type){
          if("red".equals(type)){
               return new RedPig();
          }else if("green".equals(type)){
               return new GreenPig();
          }else if("blue".equals(type)){
               return new BluePig();
          }
     }
}
```

首先针对每个Pig建立相应的工厂类，RedPigFactory,GreenPigFactory,BluePigFactory.接着将getPig方法移到子类里面,删除不需要的代码，因为各个子类工厂已经明确了自己会产生什么类，所以if条件句就多余了，type也就不需要了。特别注意，要去除static关键字，原因嘛，你懂的！！代码如下

``` {.java}
public class RedPigFactory implements PigFactory{
     public Pig getPig(){
          return new RedPig();
     }
}
```

其他两个类类似，接着将PigFactory的getPig方法改成抽象方法，或者直接将PigFactory改成接口即可。

``` {.java}
public interface PigFactory{
     public Pig getPig();
}
```

这就是工厂方法模式了。使用方法嘛，很简单，你需要什么类，就实例化哪个具体工厂调用getPig就行了。要添加新的类，那么对应新添加的类添加一个对应的工厂就OK了。这里getPig方法不能是静态的，所以建议将工厂写成单例模式。
工厂方法模式也是将创建实例的动作集中起来管理，不过比简单工厂有了几点改进：
1.符合开闭原则
2.工厂方法模式可以和模板方法模式一起使用，在创建实例的时候可以做一些其他事情。模板方法中再提。
工厂方法模式的缺点也是显而易见的，每个实例都需要一个工厂类来对应，导致工厂爆炸。所以什么时候使用，该怎么使用，还需要斟酌。

抽象工厂(Abstract Factory) :创建型模式:
---------------------------------------

从代码层面上来讲抽象工厂模式可以说是对工厂方法的进一步改进。工厂方法针对每个具体子类，都会有一个相应的子工厂。而这个子工厂只能“生产”这个子类。而抽象工厂的子工厂可以“生产”多种子类。
从逻辑层面来将，抽象工厂的子工厂所“生产”的多个子类之间需要有一定的关系，或者是相关或者是相互依赖的。
比如说，Pig不仅分颜色，还分国家，比如荷兰Pig啦，新西兰Pig啦。那么抽象工厂就可以这样写。

``` {.java}
public class RedPigFactory implements PigFactory{
     public Pig getNetherlandsPig(){
          return new RedNetherlandsPig();
     }
     public Pig getNewZealandPig(){
          return new RedNewZealandPig();
     }
}
```

依照这个类，BluePigFactory,GreenPigFactory还有PigFactory的改写应该都不是问题吧？
还有一种关系类似部分和整体的关系。比如一个工厂既生产cpu又生产内存，而cpu和内存都是电脑的一部分，两者之间也是有关系的，所以也能放到一个工厂类里面。
抽象工厂模式的难点不在代码层面，只要了解了工厂方法模式的代码，那抽象工厂模式的代码就再简单不过了。其难点在于使用上。个人认为先抽象出工厂方法模式，然后再看是否需要使用抽象工厂模式。

原型模式(Prototype) :创建型模式:
--------------------------------

原型模式的作用就是拷贝，也可以叫克隆。克隆出一个和原实例一模一样的实例。在Java中就是实现Cloneable接口。Apache的commons项目，里面有个BeanUtils，也可以实现拷贝（不过需要注意的是，BeanUtils是浅拷贝）。所以原型模式直接用就可以了。
原型模式的拷贝，可以分为浅拷贝和深拷贝，看下代码就清楚了。

``` {.java}
//浅拷贝
public class A implements Cloneable{

    private String str;
    private B b;

    public void setStr(String str){
        this.str = str;
    }

    public String getStr(){
        return str;
    }

    public B getB() {
        return b;
    }

    public void setB(B b) {
        this.b = b;
    }

    @Override
    protected Object clone(){
        A a = null;
        try {
            a = (A) super.clone();
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
        return a;
    }

    public static void main(String[] args) {
        A a = new A();
        a.setStr("Hello");
        B b = new B();
        b.setStr("Hello B");
        a.setB(b);
        System.out.println("a-str:" + a.getStr());
        System.out.println("a-b:" + a.getB().getStr());
        A ac = null;
        ac = (A) a.clone();
        System.out.println("ac-str:" + ac.getStr());
        System.out.println("ac-b:" + ac.getB().getStr());

        a.setStr("Hello A");
        b.setStr("Hello BB");
        a.setB(b);

        System.out.println("a-str:" + a.getStr());
        System.out.println("a-b:" + a.getB().getStr());
        System.out.println("ac-str:" + ac.getStr());
        System.out.println("ac-b:" + ac.getB().getStr());
    }
}

public class B{
     private String str;

    public String getStr() {
        return str;
    }

    public void setStr(String str) {
        this.str = str;
    }
}

//输出结果
/*
a-str:Hello
a-b:Hello B
ac-str:Hello
ac-b:Hello B
a-str:Hello A
a-b:Hello BB
ac-str:Hello
ac-b:Hello BB       --浅拷贝导致了，ac和a中的B是相同的引用，a中的b被修改后，ac中的b也被修改，这应该是不想要的结果
                    --所以需要深拷贝。深拷贝就是将所有涉及到的对象都进行Clone
*/
```

``` {.java}
//深拷贝
public class A implements Cloneable{

    private String str;
    private B b;

    public void setStr(String str){
        this.str = str;
    }

    public String getStr(){
        return str;
    }

    public B getB() {
        return b;
    }

    public void setB(B b) {
        this.b = b;
    }

    @Override
    protected Object clone(){
        A a = null;
        try {
            a = (A) super.clone();
            a.b = (B) b.clone();       //此处对b进行clone
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
        return a;
    }

    public static void main(String[] args) {
        A a = new A();
        a.setStr("Hello");
        B b = new B();
        b.setStr("Hello B");
        a.setB(b);
        System.out.println("a-str:" + a.getStr());
        System.out.println("a-b:" + a.getB().getStr());
        A ac = null;
        ac = (A) a.clone();
        System.out.println("ac-str:" + ac.getStr());
        System.out.println("ac-b:" + ac.getB().getStr());

        a.setStr("Hello A");
        b.setStr("Hello BB");
        a.setB(b);

        System.out.println("a-str:" + a.getStr());
        System.out.println("a-b:" + a.getB().getStr());
        System.out.println("ac-str:" + ac.getStr());
        System.out.println("ac-b:" + ac.getB().getStr());
    }
}

//B也要实现Cloneable并重写clone方法
public class B implements Cloneable{
     private String str;

    public String getStr() {
        return str;
    }

    public void setStr(String str) {
        this.str = str;
    }

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}

//输出结果
/*
a-str:Hello
a-b:Hello B
ac-str:Hello
ac-b:Hello B
a-str:Hello A
a-b:Hello BB
ac-str:Hello
ac-b:Hello B        --此处a中的b被修改后，ac中的b不会被影响
*/
```

可以直接实现Cloneable接口，而不用重写clone方法，默认就是浅拷贝。要自己实现原型模式的话也非常简单，手动在clone方法里面return一个新对象，再将自身的属性复制过去就行了，因为前面说了，原型模式基本不需要自己写了，也就不罗嗦了。

生成器模式(Builder) :创建型模式:
--------------------------------

外观模式(Facade) :结构型模式:
-----------------------------

这个模式我一直没想明白，因为太简单了。没敢把它叫模式，编码的时候经常用这模式！就是将方法组合起来。
这个模式可以说是“懒人模式”。也可以说是智能化模式。
我们以发短信为例吧。你要发短信， 第一步，需要选择联系人。
第二步，编写短信内容。 最后，发送。
iphone4S的发布将改变这一点，因为Siri。我熬夜看了iphone4S发布会，当时挺失望的。不过后来一想，如果Siri够聪明，iphone4S绝对会火。因为人懒啊。有了Siri，你只需要说:"Siri,send
a message to Ivan.Say good
morning"。然后Siri就从联系人地址簿里找到Ivan，然后发送了一条短信"Good
morning!"有没有未来科幻片的感觉？

``` {.java}
//原来发送短信
selectContract();
writeMsg(msg);
sendMsg();

//Siri发送短信
sendMsg(Voice);//搞定，当然了，里面需要语音辨析，牵扯到人工智能问题了，可比上面发送短信复杂多了，但是却方便了用户
```

Siri发送短信就是门面模式。Siri实际也是先选择联系人，填写短信，然后发送。但是都是Siri做了，Siri只是将基本的功能组合起来，提供了方便的操作。

代理模式(Proxy) :结构型模式:
----------------------------

代理模式的一种作用和代理商很类似。就像人们到商店去买东西，商店会提供一些附加服务，但是商店是不会生产东西的，商店是到工厂去拿东西。商店就是一个代理。
具体流程就像这样，用户想要一个item，于是到Shop去buyItem，而商店是不会生产item的，他就到ItemFactory去让工厂生产item，然后卖给客户，并提供售前售后服务。代码如下：

``` {.java}
//工厂接口
public interface ItemFactory{
     public Item getItem();
}

//工厂实现,Item就不实现了，随便怎么写都行
public class ItemFactoryImpl implements ItemFactory{
      public Item getItem(){
          return new Item();
      }
}

//商店类，就是代理
public class Shop implements ItemFactory{

     private ItemFactory factory = new ItemFactoryImpl();

     public Item getItem(){
          System.out.println("附加服务");
          return factory.getItem();
     } 
}

//实际调用
public class Main{
     public static void main(String[] args){
          ItemFactory f = new Shop();
          f.getItem();
     }
}
```

从代码可以看到,Shop中的getItem的getItem委托ItemFactoryImpl的getItem来生产Item，另外附加了自己的"附加服务"。
这是代理模式的一种作用。代理模式能做安全保护的作用。比如登录，如果用户名或密码不正确就不允许登录，想想怎么实现。（修改代理类里面的方法即可）

适配器模式(Adapter) :结构型模式:
--------------------------------

适配器模式的作用就是将一个类的接口转换成另外一个接口。这种模式一般是用在使用已有系统的情况。
假设目前做一个新系统，有如下代码结构。

``` {.java}
public interface Car{
     public void drive();
}

public class Benz implements Car{
     public void drive(){
          System.out.println("Benz run");
     }
}

public class Cruze implements Car{
     public void drive(){
          System.out.println("Cruze run");
     }
}
```

有一个老系统，里面有如下代码

``` {.java}
<pre class="brush:java">
public class Smart{
     public void run(){
         System.out.println("Smart run");
     }
}
```

很明显，Smart和Benz,Cruze不属于同类。而实际上Smart和Benz,Cruze一样都属于Car，怎么办呢？太简单了，继承阿。

``` {.java}
public class SmartCar extends Smart implements Car{
      public void drive(){
           this.run();
      }
}
```

搞定！So easy!这种方式叫做类的适配器。
但是，如果Car是个类，而不是接口呢？Java是不支持多继承的！依然很简单，组合嘛！

``` {.java}
public class SmartCar implements Car{
     private Smart smart = new Smart();

     public void drive(){
          smart.run();
     }
}
```

这种方式就叫做对象的适配器。很简单吧！

组合模式(Composite) :结构型模式:
--------------------------------

装饰模式(Decorator) :结构型模式:
--------------------------------

代理模式可以说是给方法添加了新内容，而装饰模式则是给类添加了新的方法。
大家都知道要添加新方法，自然会想到继承，但是继承的耦合性高。装饰模式是使用组合的方式给类添加新的方法。Java中最典型的应用就是IO库了。
JavaIO库的使用可能如下

``` {.java}
BufferedInputStream bi = new BufferedInputStream(new FileInputStream(filename));
```

我们都知道FileInputStream是用来读取文件的，BufferedInputStream是提供了缓存的能力。我们分别看下他们的源代码

``` {.java}
public class BufferedInputStream extends FilterInputStream
...

public class FilterInputStream extends InputStream
...

public class FileInputStream extends InputStream
...
```

BufferedInputStream继承了FilterInputStream，FilterInputStream和FileInputStream一样都继承自InputStream.可以看出InputStream是公共父类。FilterInputStream是装饰类的公共父类，看看FilterInputStream的源代码就知道了，他只是做了简单的方法委托。BufferedInputStream继承了FilterInputStream，并添加了缓存的方法（其实就是用一个字节数组保存字节，一次性读出）。
装饰模式的结构很简单，装饰类和被装饰类都要继承相同的父类（或实现相同的接口），装饰类持有被装饰类的实例，相同的方法只是简单的委托，在装饰类里面添加新方法即可。
装饰模式是为了解决过多且能自由组合的情况。例如HeadFirst设计模式里面举的例子，咖啡加糖，或者奶，或者即加糖也加奶，组合很多，用继承就会子类爆炸。

桥接模式(Bridge) :结构型模式:
-----------------------------

享元模式(Flyweight) :结构型模式:
--------------------------------

模板模式(Template) :行为型模式:
-------------------------------

在工厂方法模式里提到了工厂方法模式可以和模板方法模式一起使用，在创建实例的时候可以做一些其他事情。我们就在工厂方法getPig的时候打印点东西吧。
该怎么修改呢？非常简单，只需要修改接口就可以了，子类不需要改动。

``` {.java}
public interface PigFactory{
     public Pig getPig();
}
```

要在getPig的前后打印输出，那就在getPig调用的前后加上打印操作就可以了，新建个方法createPig，记得把接口改成抽象类。

``` {.java}
public abstract class PigFactory{

     public Pig createPig(){
          System.out.println("Start");
          getPig();
          System.out.println("End");
     }

     public abstract Pig getPig();
}
```

搞定。当然了，原来调用getPig的地方要改成调用createPig了。
从代码就可以看出来，模板方法应用在大框架不变，但是有细节会有变化的情况下。所以框架类代码应用比较多。
模板方法模式应该是对“把变化的东西和不变的东西分离开来”的最好诠释。

备忘录模式(Memento) :行为型模式:
--------------------------------

观察者模式(Observer) :行为型模式:
---------------------------------

Observer直译为中文为观察者。所以此模式又称为观察者模式（废话。）不过一直很疑惑，我感觉这个名字起得不好，给人误解很大，特别是初学者。我想大部分人的想法应该和我一样，我第一次看到观察者模式就会想到，观察者观察者，那么肯定观察者是主体了。其实不然。被观察者才是主体。
就小孩睡觉这个例子，如果按观察者的想法，那就观察嘛！！！怎么观察，小孩在睡觉，我就在那观察，他眼一睁，啊，要喝奶了。赶紧去泡奶粉。。。。这个显然就不正常。傻子才这么干。。。。
实际上是你该干嘛就干嘛去，小孩醒了，一哭要喝奶了，你就去泡奶吧。
所以我认为观察者模式叫Call模式或者Call-Response模式更恰当。
下面看程序，模拟找工作。。。。雇主通知他要招聘的雇员，雇员得到通知后做出响应。
先是两个接口，Employer和Employee。

``` {.java}
package pig.pattern.Observer;

/**
 * Created by IntelliJ IDEA.
 * User: Administrator
 * Date: 2008-8-28
 * Time: 14:31:33
 * To change this template use File | Settings | File Templates.
 */
public interface Employer {
    void agree_call();           
    void agree(Employee employee);  //相当于swing里的add***Listener()
}
```

``` {.java}
package pig.pattern.Observer;

/**
 * Created by IntelliJ IDEA.
 * User: Administrator
 * Date: 2008-8-28
 * Time: 14:32:14
 * To change this template use File | Settings | File Templates.
 */
public interface Employee {
    void response(EmployEvent event);
//这个相当于ActiongListener里面的actionPerformed(ActionEvent event)
}
```

再来相应的实现类

``` {.java}
package pig.pattern.Observer;

import java.util.List;
import java.util.ArrayList;

/**
 * Created by IntelliJ IDEA.
 * User: Administrator
 * Date: 2008-8-28
 * Time: 14:32:51
 * To change this template use File | Settings | File Templates.
 */
public class Employer_1 implements Employer{
    private List<Employee> employees = new ArrayList<Employee>();
    private EmployEvent event;

    public void agree(Employee employee){
        employees.add(employee);
    }

    public void agree_call() {
        event = new EmployEvent();
        event.setEmployer_name(this.getClass().getSimpleName());
        for(Employee e : employees){
            e.response(event);
        }
    }
}
```

``` {.java}
package pig.pattern.Observer;

/**
 * Created by IntelliJ IDEA.
 * User: Administrator
 * Date: 2008-8-28
 * Time: 14:49:22
 * To change this template use File | Settings | File Templates.
 */
public class Employee_1 implements Employee{
    public void response(EmployEvent event) {
        System.out.println("Oh,I'm lucky.I'm employed by " + event.getEmployer_name());
    }
}
```

``` {.java}
package pig.pattern.Observer;

/**
 * Created by IntelliJ IDEA.
 * User: Administrator
 * Date: 2008-8-28
 * Time: 14:49:22
 * To change this template use File | Settings | File Templates.
 */
public class Employee_2 implements Employee{
    public void response(EmployEvent event) {
        System.out.println("Yeah,I'm employed by " + event.getEmployer_name());
    }
}
```

再来事件类

``` {.java}
package pig.pattern.Observer;

/**
 * Created by IntelliJ IDEA.
 * User: Administrator
 * Date: 2008-8-28
 * Time: 14:39:48
 * To change this template use File | Settings | File Templates.
 */
public class EmployEvent {
    private String employer_name;

    public String getEmployer_name() {
        return employer_name;
    }

    public void setEmployer_name(String employer_name) {
        this.employer_name = employer_name;
    }
}
```

``` {.java}
package pig.pattern.Observer;

/**
 * Created by IntelliJ IDEA.
 * User: Administrator
 * Date: 2008-8-28
 * Time: 14:51:00
 * To change this template use File | Settings | File Templates.
 */
public class Test {
    public static void main(String[] args){
        Employer e_1 = new Employer_1();
        e_1.agree(new Employee_1());
        e_1.agree(new Employee_2());
        e_1.agree_call();
    }
}
```

Employer在众多的简历里面挑选出自己满意的，然后agree到自己的通知列中(list)，全部都确认后，然后一起去call所有的这些应聘者，各个应聘者做出不同的反应。EmployEvent就相当于通知书一样，里面保存了Employer的名称，不然你怎么知道你是被哪个Employer聘了？
这还是说明，观察者(Employee)并不是主体，被观察者(Employer)才是主体。如果Employee是主体，那是不是你投完简历以后就天天跟着那个Employer，他一天不做出决定，你就一天不停了？
至于用到哪儿，根据代码应该就能猜得出来。一个对象的改变会影响其他对象的响应，而且对象个数不确定。
很粗糙的代码，只想说别被名字唬到了。。。。
我更愿意称观察者模式为通知模式，我觉得“通知模式”更能反应这个模式的主旨----你该干嘛干嘛去，有事我通知你！
此模式的最佳应用在哪里呢？3Q大战。我们来模拟一下！

``` {.java}
//QQ Client
public interface QQ{
     public boolean login(String username,String password);
     public void popWin(String msg);
}

//QQ Server
public interface QQServer{

    private List<QQ> list = new ArrayList(); 
    public void addQQ(QQ qq){
         list.add(qq);
    }

    public void popAll(String msg){
        for(QQ qq : list) qq.popWin(msg);
    }
}

//QQ实现
public class MyQQ implements QQ{
     public boolean login(String username,String password){
           //这里使用网络发送请求到QQServer，如果用户名密码正确，就调用addQQ方法,于是我们的QQ就上战场了
     }

     public void popWin(String msg){
          System.out.println(msg);
     }
}
```

这样，只要QQServer一做出"艰难的决定"，我们就都遭殃了！
当然，这只是一个模拟，真实代码就不得而知了！在Web中，这种模式也叫做“消息推送”。相对的客户端定时向服务器请求，叫“拉”模式。

职责链模式(Chain of Responsibility) :行为型模式:
------------------------------------------------

责任链模式的典型应用就是过滤器！我们来模拟一下：

``` {.java}
//模拟Request和Response类
public class Request{
    private HashMap<String,String> map = new HashMap<String,String>();

    public void setAttribute(String key,String value){
         map.put(key,value);
    }

    public String getAttribute(String key){
        return  map.get(key);
    }

    public String toString(){
        String return_str = "";
        Set key = map.keySet();
         Iterator i = key.iterator();
         while (i.hasNext()){
             return_str = return_str + "=====" + map.get(i.next());
         }
         return return_str;
    }
}

public class Response{
    private HashMap<String,String> map = new HashMap<String,String>();

    public void setAttribute(String key,String value){
         map.put(key,value);
    }

    public String getAttribute(String key){
        return  map.get(key);
    }

    public String toString(){
        String return_str = "";
        Set key = map.keySet();
         Iterator i = key.iterator();
         while (i.hasNext()){
             return_str = return_str + "=====" + map.get(i.next());
         }
         return return_str;
    }
}

//Filter接口及其实现类
public interface Filter{
    public void doFilter(Request request,Response response,FilterChain chain);
}

public class CheckFilter implements Filter{
    @Override
    public void doFilter(Request request,Response response,FilterChain chain){
          request.setAttribute("name",request.getAttribute("name")+"Check");
          chain.doFilter(request,response,chain);
          response.setAttribute("name",response.getAttribute("name")+"Check");
    }
}

public class LoginFilter implements Filter{

    @Override
    public void doFilter(Request request,Response response,FilterChain chain){
          request.setAttribute("name",request.getAttribute("name")+"Login");
          chain.doFilter(request,response,chain);
          response.setAttribute("name",response.getAttribute("name")+"Login");
    }
}

//FilterChain维护了一个Filter的链
public class FilterChain {

    private LinkedList<Filter> list = new LinkedList<Filter>();
    private int index = 0;


    public FilterChain addFilter(Filter filter){
        list.add(filter);
        return this;
    }

    public void doFilter(Request request,Response response,FilterChain chain){
        if(index == list.size()) {
            return;
        }

        Filter f = list.get(index);
        index ++;
        f.doFilter(request, response, chain);
    }
}

//测试
public class  ChainTest{

    @Before
    public void init(){

    }

    @Test
    public void testChain(){
        Request r = new Request();
        Response p = new Response();
        r.setAttribute("name","Ivan");
        p.setAttribute("name","Ivan");

        FilterChain chain = new FilterChain();
        chain.addFilter(new LoginFilter()).addFilter(new CheckFilter());

        chain.doFilter(r,p,chain);

        System.out.println(r.toString());
        System.out.println(p.toString());
    }
}
```

如上代码是责任链模式的一个变种。你可以去除FilterChain类，在每个Filter里面维护一个Filter实例，指向下一个Filter（链表的结构），在doFilter中进行判断。这样就是一个基本的责任链模式了。

命令模式(Command) :行为型模式:
------------------------------

状态模式(State) :行为型模式:
----------------------------

状态模式和策略模式很像，不过状态模式多了一个状态的改变。可以说，策略模式给了你积木，让你随意的搭积木，而状态模式多了一些限制，就是一个积木后面要跟另一个积木，而不能随意搭。
相信大家都在网上买过东西，买东西的流程：选择商品-》付款-》卖家发货-》买家确认-》完成。这其中对应了订单状态的变化：未付款-》已付款-》已发货-》完成交易。代码如下：

``` {.java}
public class Order{
     private static int UN_PAY = 0;
     private static int PAYED = 1;
     private static int SEND = 2;
     private static int DONE = 3;    //四种状态，可以使用枚举

     private int state = UN_PAY;     //当前状态

     public void pay(){
          if(state = UN_PAY){
               pay money;
               state = PAYED;
          }else{
               exception;
          }
     }

     public void send(){
             .....        //同样的if,else
     }

     ....
}
//可以看出如上代码充斥了很多的if,else，当状态更多时，此类膨胀很厉害，且难以维护
//状态模式将各个状态封装成一个个的类，并做该状态下可以做的事情，Order只是简单的委托。
public class Order{
     private State unPayState;
     private State payedState;
     private State sendState;
     private State doneState;    //四种状态，四种类,对应的get,set方法省略

     private State state = unPayState;     //当前状态

     public Order(){
        unPayState = new UnPayState(this);
        payedState = new PayedState(this);            //传入Order对象，为了修改Order的状态
        ...
     }

     public void pay(){
          state.pay();
     }

     public void send(){
          state.send();
     }

     ....
}

//State接口
public interface State{
     public void pay();
     public void send();
     ...
}

//UnPayState
public class UnPayState implements State{

     private Order order;
     public UnPayState(Order order){
           this.order = order;
     }

     public void pay(){
         //pay money
         order.setState(order.getPayedState());
     }

     public void send(){
         //exception: can't send
     }
     .....
}
```

------------------------------------------------------------------------

状态模式(State)
定义：当一个对象的内在状态改变时允许改变其行为，这个对象看起来像是改变了其类。
用途：主要解决当控制一个对象状态转换的条件表达式过于复杂时的情况。把状态的判断逻辑转移到表示不同状态的一系列类当中，可以把复杂的判断逻辑简化！

定义依然的让人迷惑不解！如果说，状态模式就是为了替换复杂的if...else语句的，是否会有点恍然大悟的感觉？
我现在对状态模式的理解是这样的，一个类里面有过于复杂的if...else，维护起来很不方便，如果哪里有问题了，要维护一大块代码，而如果有新的条件了，则需要修改这一大段的代码。状态模式就是为了解决这个问题的。
假设有如下一段代码。

``` {.java}
public class Find {
    public void find(int id) {
        if (id == 1) {
            System.out.println("1");
        } else if (id == 2) {
            System.out.println("2");
        } else if (id == 3) {
            System.out.println("3");
        } else if (id == 4) {
            System.out.println("4");
        }
    }
}
```

如果判断里面的业务比较麻烦的话，那么这段代码就很复杂了。而如果可能需求又变了，还要判断id
== 5,又要找到这段代码来修改。
看看状态模式的写法，在这里，一个判断就是一个所谓的状态，符合这个状态就执行相应的动作，否则就进入下一个状态。可以想象，每个状态都有统一的接口。

``` {.java}
public interface State {
    public void execute(Find find,int id);
}
```

修改Find

``` {.java}
public class Find {
    private State state;

    public void setState(State state) {
        this.state = state;
    }

    public Find(State state) {
        this.state = state;
    }

    public void stateFind(int id) {
        state.execute(this, id);
    }
}
```

再看State的子类怎么写。

``` {.java}
public class State1 implements State{
    public void execute(Find find,int id) {
        if(id == 1){
            System.out.println("1");
        }else{
            find.setState(new State2());
            find.stateFind(id);
        }
    }
}

public class State2 implements State {
    public void execute(Find find, int id) {
        if(id == 2){
            System.out.println("2");
        }else{
            find.setState(new State3());
            find.stateFind(id);
        }
    }
}

public class State3 implements State {
    public void execute(Find find, int id) {
        if(id == 3){
            System.out.println("3");
        }else{
            find.setState(new State3());
            find.stateFind(id);
        }
    }
}
```

测试

``` {.java}
public class FindTest {
    Find find;

    @Before
    public void init(){
        find = new Find(new State1());
    }

    @Test
    public void testFind() throws Exception {
        find.stateFind(3);
    }
}
```

如果需要修改各个状态所对应的行为，到相应的类里面就可以了。如果要添加状态，实现State接口，然后依次加上去即可。类似链表了。
缺点显而易见，类太多了。。。。

策略模式(Strategy)：它定义了算法家族，分别封装起来，让它们之间可以互相替换，此模式让算法的变化，不会影响到使用算法的客户。
用途：在不同时间需要应用不同的业务规则的时候，就可以考虑使用策略模式。

这个定义，我依然理解不了（可能智商不够高 ）

我的理解是，就代码上而言，策略模式和状态模式区别很小。状态模式是在内部判断，而策略模式是外部传入行为，直接去执行行为，而不需要再判断状态了。

修改代码如下：

``` {.java}
public interface State {
    public void execute();
}
```

``` {.java}
public class State1 implements State {
    public void execute() {
        System.out.println("1");
    }
}

public class State2 implements State {
    public void execute() {
        System.out.println("2");
    }
}

public class State3 implements State {
    public void execute() {
        System.out.println("3");
    }
}
```

``` {.java}
public class Find {
    private State state;

    public Find(State state) {
        this.state = state;
    }

    public void stateFind() {
        state.execute();
    }
｝
```

测试

``` {.java}
public class FindTest {
    Find find;

    @Before
    public void init(){

    }

    @Test
    public void testTotal(){
        find = new Find(new State1());
        find.stateFind();
        find = new Find(new State2());
        find.stateFind();
        find = new Find(new State3());
        find.stateFind();
    }
}
```

策略模式(Strategy) :行为型模式:
-------------------------------

公司要建立一个模拟系统，很简单，就是模拟鸭子Quake和Fly，目前只有一只红鸭子。那么设计就非常简单了。我们使用TDD来开发。既然红鸭子能Quake和Fly，那么我们就有一个RedDuckTest，以及测试testQuake,testFly。
代码如下：

``` {.java}
public class RedDuckTest {

    private RedDuck rDuck;

    @Before
    public void init(){
        rDuck = new RedDuckTest();
    }

    @Test
    public void testQuake() throws Exception {
        assertEquals("RedDuck GaGa",rDuck.quake());
    }

    @Test
    public void testFly() throws Exception {
        assertEquals("RedDuck Flying",rDuck.fly());
    }

    @After
    public void destroy(){
    }
}
```

这段测试代码展示了RedDuck的实现需求。但是你会发现很多红叉叉，这是肯定的，因为你还没有RedDuck以及quake和fly方法。分别将鼠标定位到RedDuck,quake和fly上，按ctrl+1回车即可自动创建出相应的代码(eclipse，其他IDE快捷键请参考IDE帮助)。创建完的类如下所示。

``` {.java}
public class RedDuck {

    public String quake() {
        return null;
    }

    public String fly() {
        return null;
    }
}
```

原来测试类也没有错误了，运行测试，测试不通过。很明显，因为返回不符合要求。改一下就OK了。

``` {.java}
public class RedDuck {

    public String quake() {
        return "RedDuck GaGa";
    }

    public String fly() {
        return "RedDuck Flying";
    }
}
```

现在又有了新的需求了，要只绿鸭子。同样先写测试,再ctrl+1。

``` {.java}
public class GreenDuckTest {

    private GreenDuck gDuck;

    @Before
    public void init(){
        gDuck = new GreenDuck();
    }

    @Test
    public void testQuake() throws Exception {
        assertEquals("GreenDuck GaGa",gDuck.quake());
    }

    @Test
    public void testFly() throws Exception {
        assertEquals("GreenDuck Flying",gDuck.fly());
    }

    @After
    public void destroy(){
    }
}
```

``` {.java}
public class GreenDuck {

    public String quake() {
        return "GreenDuck GaGa";
    }

    public String fly() {
        return "GreenDuck Flying";
    }
}
```

问题来了，GreenDuck和RedDuck的方法完全一样，那么可以抽出一个接口出来了，命名为Duck。

------------------------------------------------------------------------

策略模式的结构很简单，其实就是面向接口编程。具体是对其的理解。

![](files/Strategy.gif)
我对策略模式的理解是：策略模式将一个个的具体策略（逻辑、算法或行为）封装到了一个个的子类中，而由用户（客户端）自己决定调用哪个策略！换一种说法，策略模式将逻辑从服务端提到了客户端，本来由服务器端来判断的事情交由客户端自己决定，服务器端只提供应有的功能。就像给你积木，让你自己搭积木一样。
举个简单的例子，写个简单计算器，可能会这么写：

``` {.java}
//判断符号，来决定做怎样的计算。这就导致了，当添加新计算逻辑的时候，此类要不断维护
if(type.equals("+")) return a+b;
else if(type.equals("-")) return a-b;
....

//而策略模式将加减乘除封装到一个个的类中
public class AddStrategy implements Strategy{
     public double caculate(double a,double b){
           return a + b;
     }
}

//当调用时，比如当按下了界面上的+按钮，则将AddStrategy设置给客户类
client.setStrategy(new AddStrategy());
return client.result();    //result方法委托caculate方法即可
```

当需要再添加新算法时，添加相应的子类即可（当然了，客户端还是要修改的）！

中介者模式(Mediator) :行为型模式:
---------------------------------

解释器模式(Interpreter) :行为型模式:
------------------------------------

访问者模式(Visitor) :行为型模式:
--------------------------------

### 定义

作用于某个对象群中各个对象的操作.
它可以使你在不改变这些对象本身的情况下,定义作用于这些对象的新操作.

迭代器模式(Iterator) :行为型模式:
---------------------------------

和原型模式一样，迭代器模式也基本不需要自己实现了，Java中提供了Iterator接口，可直接实现迭代器。迭代器模式也非常简单，就是为不同的集合统一遍历接口。
这里看下Java中迭代器的实现代码，以ArrayList为例。

``` {.java}
//ArrayList实现了List接口
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable

//List继承了Collection
public interface List<E> extends Collection<E>

//Collection继承了Iterable接口
public interface Collection<E> extends Iterable<E>

//Iterable只有一个方法，就是返回Iterator对象，Iterator对象的作用就是对List的遍历
 public interface Iterable<T> {
    Iterator<T> iterator();
}

//ArrayList里面实现了此iterator()方法，如下
 public Iterator<E> iterator() {
        return new Itr();
    }

//而Itr是ArrayList的内部类，且这个内部类实现了Iterator接口，实现对ArrayList遍历的具体方法
 private class Itr implements Iterator<E> {
        int cursor;       // index of next element to return
        int lastRet = -1; // index of last element returned; -1 if no such
        int expectedModCount = modCount;

        public boolean hasNext() {
            return cursor != size;
        }

        @SuppressWarnings("unchecked")
        public E next() {
            checkForComodification();
            int i = cursor;
            if (i >= size)
                throw new NoSuchElementException();
            Object[] elementData = ArrayList.this.elementData;
            if (i >= elementData.length)
                throw new ConcurrentModificationException();
            cursor = i + 1;
            return (E) elementData[lastRet = i];
        }

        public void remove() {
            if (lastRet < 0)
                throw new IllegalStateException();
            checkForComodification();

            try {
                ArrayList.this.remove(lastRet);
                cursor = lastRet;
                lastRet = -1;
                expectedModCount = modCount;
            } catch (IndexOutOfBoundsException ex) {
                throw new ConcurrentModificationException();
            }
        }

        final void checkForComodification() {
            if (modCount != expectedModCount)
                throw new ConcurrentModificationException();
        }
    }
```

所以整体结构是这样的。集合类实现了Iterable接口，返回一个Iterator对象，这个对象针对此集合类实现遍历方法。

