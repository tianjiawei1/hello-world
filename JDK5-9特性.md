### JDK1.5新特性

##### 1.自动装箱与拆箱

自动装箱就是Java自动将原始类型值转换成对应的对象，比如将int的变量转换成Integer对象，这个过程叫做装箱，反之将Integer对象转换成int类型值，这个过程叫做拆箱。因为这里的装箱和拆箱是自动进行的非人为转换，所以就称作为自动装箱和拆箱。

```java
原始类型byte,short,char,int,long,float,double和boolean
对应的封装类为Byte,Short,Character,Integer,Long,Float,Double,Boolean。

// 手动拆装箱 - before autoboxing 
Integer iObject = Integer.valueOf(3); 
int iPrimitive = iObject.intValue(); 

// java5之后 - after java5 
Integer iObject = 3; 

// 自动拆装箱 - primitive to wrapper conversion 
int iPrimitive = iObject;
```

##### 2.枚举

创建枚举类型要使用 enum 关键字，隐含了所创建的类型都是 java.lang.Enum 类的子类（java.lang.Enum 是一个抽象类）。枚举类型符合通用模式 Class Enum

```java
package com.xxx.test; 

public enum EnumTest { 
MON, TUE, WED, THU, FRI, SAT, SUN; 
} 

// 上面这段代码实际上调用了7次 Enum(String name, int ordinal)： 
new Enum<EnumTest>("MON",0); 
new Enum<EnumTest>("TUE",1);
new Enum<EnumTest>("WED",2); ...
```

##### 3.静态导入（static import）

静态导入使代码更易读。通常，你要使用定义在另一个类中的常量（constants）时，需要使用静态导入来导入这个类或者常量。

```java
// 静态导入枚举MON 

import static com.xxx.test.EnumTest.MON; 
// 此处可以直接使用枚举 
	switch (MON) { 
		case MON: 
		break; 
		case TUE:
		break; 
		default: 
		break; 
	}
```

4. ##### 可变参数（Varargs）

   https://blog.csdn.net/qiuchengjia/article/details/52910888

##### 5.内省（Introspector）

主要用于操作JavaBean中的属性，通过getXxx/setXxx。一般的做法是通过类Introspector来获取某个对象的BeanInfo信息，然后通过BeanInfo来获取属性的描述器（PropertyDescriptor），通过这个属性描述器就可以获取某个属性对应的getter/setter方法，然后我们就可以通过反射机制来调用这些方法。

##### 6.泛型(Generic)（包括通配类型/边界类型等）

通过引入泛型，我们将获得编译时类型的安全和运行时更小地抛出ClassCastExceptions的可能。

```java
// new HashMap<String, Object> 此处需要指定map的类型
 Map<String, Object> map = new HashMap<String, Object>(2);
```

##### 7.ForEach循环  更简洁的for循环

```java
// 这种for循环更具可读性 

for (Object obj: Objects) { 
// code
 }
```

##### 8.协变返回类型

实际返回类型可以是要求的返回类型的一个子类型

在面向对象程序设计中，协变返回类型指的是子类中的成员函数的返回值类型不必严格等同于父类中被重写的成员函数的返回值类型，而可以是更 "狭窄" 的类型。

Java 5.0添加了对协变返回类型的支持，即子类覆盖（即重写）基类方法时，返回的类型可以是基类方法返回类型的子类。协变返回类型允许返回更为具体的类型。

```java
import java.io.ByteArrayInputStream;
import java.io.InputStream;
class Base
{
    //子类Derive将重写此方法，将返回类型设置为InputStream的子类
  public InputStream getInput()
  {
    　　return System.in;
  }
}
public  class Derive extends Base
{
    @Override
    public ByteArrayInputStream getInput()
    {       
        return new ByteArrayInputStream(new byte[1024]);
    }
    public static void main(String[] args)
    {
        Derive d=new Derive();
        System.out.println(d.getInput().getClass());
    }
}
/*程序输出：
class java.io.ByteArrayInputStream
*/
```

##### 9.元数据

元数据是关于数据的数据。在编程语言上下文中，元数据是添加到程序元素如方法、字段、类和包上的额外信息，对数据进行说明描述的数据；元数据特征志于使开发者们借助厂商提供的工具可以进行更简易的开发。

**注解**Annotation就是Java平台的元数据，是 J2SE5.0新增加的功能，该机制允许在Java 代码中添加自定义注释，并允许通过反射（reflection），以编程方式访问元数据注释。通过提供为程序元素（类、方法等）附加额外数据的标准方法，元数据功能具有简化和改进许多应用程序开发领域的潜在能力，其中包括配置管理、框架实现和代码生成。

在程序开发中，我们常用的注解有@Service、@Override、@Autowired、@Controller…

```java
// 注解 

public @interface MyAnnotation {

     // 定义公共的final静态属性

    int age = 24;  

// 定义公共的抽象方法 

String name(); 

}
```

##### 10.线程池

有关Java5线程新特征的内容全部在java.util.concurrent下面，里面包含数目众多的接口和类。

java.uitl.concurrent.ThreadPoolExecutor类是线程池中最核心的一个类，因此如果要透彻地了解Java中的线程池，必须先了解这个类。下面我们来看一下ThreadPoolExecutor类的具体实现源码。

 

// 在ThreadPoolExecutor类中提供了四个构造方法：

```java
 public class ThreadPoolExecutor extends AbstractExecutorService {

    ..... 

public ThreadPoolExecutor(int corePoolSize,int maximumPoolSize,long keepAliveTime,TimeUnit unit, BlockingQueue<Runnable> workQueue);

 public ThreadPoolExecutor(int corePoolSize,int maximumPoolSize,long keepAliveTime,TimeUnit unit, BlockingQueue<Runnable> workQueue,ThreadFactory threadFactory); 

public ThreadPoolExecutor(int corePoolSize,int maximumPoolSize,long keepAliveTime,TimeUnit unit, BlockingQueue<Runnable> workQueue,RejectedExecutionHandler handler); 

public ThreadPoolExecutor(int corePoolSize,int maximumPoolSize,long keepAliveTime,TimeUnit unit, BlockingQueue<Runnable> workQueue,ThreadFactory threadFactory,RejectedExecutionHandler handler);

 ...

 }
```

从上面的代码可以得知，ThreadPoolExecutor继承了AbstractExecutorService类，并提供了四个构造器，事实上，通过观察每个构造器的源码具体实现，发现前面三个构造器都是调用的第四个构造器进行的初始化工作。

下面解释下一下构造器中各个参数的含义：

\* corePoolSize：核心池的大小，这个参数跟后面讲述的线程池的实现原理有非常大的关系。在创建了线程池后，默认情况下，线程池中并没有任何线程，而是等待有任务到来才创建线程去执行任务，除非调用了prestartAllCoreThreads()或者prestartCoreThread()方法，从这2个方法的名字就可以看出，是预创建线程的意思，即在没有任务到来之前就创建corePoolSize个线程或者一个线程。默认情况下，在创建了线程池后，线程池中的线程数为0，当有任务来之后，就会创建一个线程去执行任务，当线程池中的线程数目达到corePoolSize后，就会把到达的任务放到缓存队列当中；

 \* maximumPoolSize：线程池最大线程数，这个参数也是一个非常重要的参数，它表示在线程池中最多能创建多少个线程； 

\* keepAliveTime：表示线程没有任务执行时最多保持多久时间会终止。默认情况下，只有当线程池中的线程数大于corePoolSize时，keepAliveTime才会起作用，直到线程池中的线程数不大于corePoolSize，即当线程池中的线程数大于corePoolSize时，如果一个线程空闲的时间达到keepAliveTime，则会终止，直到线程池中的线程数不超过corePoolSize。但是如果调用了allowCoreThreadTimeOut(boolean)方法，在线程池中的线程数不大于corePoolSize时，keepAliveTime参数也会起作用，直到线程池中的线程数为0； 

\* unit：参数keepAliveTime的时间单位，有7种取值，在TimeUnit类中有7种静态属性： 

```java
TimeUnit.DAYS; // 天 
TimeUnit.HOURS; // 小时 
TimeUnit.MINUTES; // 分钟 
TimeUnit.SECONDS; // 秒 
TimeUnit.MILLISECONDS; // 毫秒 
TimeUnit.MICROSECONDS; // 微妙 
TimeUnit.NANOSECONDS; // 纳秒 
```

\* workQueue：一个阻塞队列，用来存储等待执行的任务，这个参数的选择也很重要，会对线程池的运行过程产生重大影响，一般来说，这里的阻塞队列有以下几种选择：

```java
ArrayBlockingQueue; 
LinkedBlockingQueue;
SynchronousQueue; 
```

ArrayBlockingQueue和PriorityBlockingQueue使用较少，一般使用LinkedBlockingQueue和Synchronous。线程池的排队策略与BlockingQueue有关。 

\* threadFactory：线程工厂，主要用来创建线程； 

\* handler：表示当拒绝处理任务时的策略，有以下四种取值： 

ThreadPoolExecutor.AbortPolicy:丢弃任务并抛出RejectedExecutionException异常。

    ThreadPoolExecutor.DiscardPolicy：也是丢弃任务，但是不抛出异常。
    
    ThreadPoolExecutor.DiscardOldestPolicy：丢弃队列最前面的任务，然后重新尝试执行任务（重复此过程） 

ThreadPoolExecutor.CallerRunsPolicy：由调用线程处理该任务

##### 11.Jaca Generics

对于一个Collection类库中的容器类实例，可将任意类型对象加入其中（都被当作Object实例看待）；从容器中取出的对象也只是一个Object实例，需要将其强制转型为期待的类型，这种强制转型的运行时正确性由程序员自行保证。 JDK1.5中Collection类库的大部分类都被改进为Generic类。

对于程序中的使用，俺就举一个自己封装的栗子吧；在开发中，我们总会发现，自己重复性的调用一些功能一摸一样的mapper和service方法，比如，根据ID查询当前对象实体、根据ID删除当前对象、分页查询等…；这时，就轮到我们的Generics泛型登场了。

```java
package com.xxx.common.entity; 

public abstract class BaseEntity extends Entity { 
	private static final long serialVersionUID = xxxL; 
	protected String orderBy; 
	rivate String version = "1.0.0"; 
}
 
package com.xxx.common.dao; 

import com.github.pagehelper.Page; 
import com.xxx.common.entity.BaseEntity; 
import java.io.Serializable; 
import java.util.List; 
import java.util.Map; 

public interface BaseMapper<T extends BaseEntity, ID extends Serializable> {

int createEntity(T var1); 
int updateEntity(T var1); 
int deleteById(ID var1); 
T queryById(ID var1); 
List<T> queryListByEntity(T var1); 
Page<T> pageQueryEntity(T var1); 
long pageQueryEntityCount(T var1); 
}
```

### JDK1.6新特性

代码：https://blog.csdn.net/yclimb/article/details/7921137

##### 1.AWT新增加了两个类:Desktop和SystemTray

其中前者用来通过系统默认程序来执行一个操作，如

使用默认浏览器浏览指定的URL,

用默认邮件客户端给指定的邮箱发邮件,

用默认应用程序打开或编辑文件(比如,用记事本打开以txt为后缀名的文件),

用系统默认的打印机打印文档等。

后者可以用来在系统托盘区创建一个托盘程序

##### 2.使用JAXB2来实现对象与XML之间的映射，

可以将一个Java对象转变成为XML格式，反之亦然

我们把对象与关系数据库之间的映射称为ORM, 其实也可以把对象与XML之间的映射称为OXM(Object XML Mapping). 原来JAXB是Java EE的一部分，在JDK6中，SUN将其放到了Java SE中，这也是SUN的一贯做法。JDK6中自带的这个JAXB版本是2.0, 比起1.0(JSR 31)来，JAXB2(JSR 222)用JDK5的新特性Annotation来标识要作绑定的类和属性等，这就极大简化了开发的工作量。实际上，在Java EE 5.0中，EJB和Web Services也通过Annotation来简化开发工作。另外,JAXB2在底层是用StAX(JSR 173)来处理XML文档。

##### 3.StAX

一种利用拉模式解析(pull-parsing)XML文档的API。类似于SAX，也基于事件驱动模型。

StAX是JDK1.6中除了DOM和SAX之外的又一种处理XML文档的API。

StAX是The Streaming API for XML的缩写。由于JDK6.0中的JAXB2和JAX-WS 2.0都会用到StAX，所以Sun决定把StAX加入到JAXP家族当中来，并将JAXP的版本升级到1.4.

JDK6里面JAXP的版本就是1.4。JAXP是Java API for XML Processing的英文字头缩写,中文含义是:用于XML文档处理的使用Java语言编写的编程接口。

博客：http://www.blogjava.net/hsith/archive/2006/06/29/55817.html

##### 4.使用Compiler API

动态编译Java源文件，如JSP编译引擎就是动态的，所以修改后无需重启服务器。

可以用JDK1.6 的Compiler API动态编译Java源文件，Compiler API结合反射功能就可以实现动态的产生Java代码并编译执行这些代码，有点动态语言的特征。

这个特性对于某些需要用到动态编译的应用程序相当有用， 比如JSP Web Server，当我们手动修改JSP后，是不希望需要重启Web Server才可以看到效果的，这时候我们就可以用Compiler API来实现动态编译JSP文件。

博客：https://blog.csdn.net/zhongweijian/article/details/7619396

 

##### 5.轻量级Http Server API

据此可以构建自己的嵌入式HttpServer,它支持Http和Https协议。

JDK6 提供了一个简单的Http Server API,据此我们可以构建自己的嵌入式Http Server,它支持Http和Https协议,提供了HTTP1.1的部分实现，没有被实现的那部分可以通过扩展已有的Http Server API来实现。

博客：https://blog.csdn.net/earbao/article/details/41211041

##### 6.插入式注解处理API(PluggableAnnotation Processing API)

JSR （JSR是Java Specification Requests的缩写，意思是Java 规范请求）用Annotation Processor在编译期间而不是运行期间处理Annotation, Annotation Processor相当于编译器的一个插件,所以称为插入式注解处理。

博客：https://blog.csdn.net/chinajash/article/details/1471081

##### 7.提供了Console类用以开发控制台程序

位于java.io包中。据此可方便与Windows下的cmd或Linux下的Terminal等交互。

JDK6中提供了java.io.Console 类专用来访问基于字符的控制台设备. 你的程序如果要与Windows下的cmd或者Linux下的Terminal交互,就可以用Console类代劳。

博客：https://blog.csdn.net/chinajash/article/details/1474216

##### 8.对脚本语言的支持

如: ruby,groovy, javascript

博客：https://blog.csdn.net/gladmustang/article/details/41620849

##### 9.Common Annotations

Common annotations原本是Java EE 5.0规范的一部分，现在SUN把它的一部分放到了Java SE 6.0中. 随着Annotation元数据功能加入到Java SE 5.0里面，很多Java 技术(比如EJB,Web Services)都会用Annotation部分代替XML文件来配置运行参数，保证Java SE和Java EE 各种技术的一致性。

博客：https://blog.csdn.net/chinajash/article/details/1479964

##### 10.嵌入式数据库 Derby

**博客：** **https://blog.csdn.net/iteye_3224/article/details/81527802**



### JDK1.7 新特性

##### 1.对Java集合（Collections）的增强支持

可直接采用[]、{}的形式存入对象，采用[]的形式按照索引、键值来获取集合中的对象。如：

```java
List<String>list=[“item1”,”item2”];//存
Stringitem=list[0];//直接取
Set<String>set={“item1”,”item2”,”item3”};//存
Map<String,Integer> map={“key1”:1,”key2”:2};//存
Intvalue=map[“key1”];//取
```

在网上的例子中，下面这段代码一般用来表示JDK7新特性语法上支持集合，然而经过验证，这是一个错误的描述，JDK7并没有这个特性；用JDK8来测试都报语法错误，请大家注意不要上当！！！

```java
final List<Integer> piDigits = [ 1,2,3,4,5,8 ];
```

##### 2.在Switch中可用String

```java
String s = "test"; 
switch (s) { 
case "test": 
System.out.println("test"); 
case "test1": 
System.out.println("test1"); 
break; 
default: // 阿里巴巴开发规范中，默认都需要加default System.out.println("default"); 
break; 
}
```

##### 3.数值可加下划线用作分隔符（编译时自动被忽略）

##### 4.二进制变量的表示,支持将整数类型用二进制来表示，用0b开头。

```java
// 所有整数 int， short，long，byte都可以用二进制表示 

// An 8-bit 'byte' value:
 byte aByte = (byte) 0b00100001; 

// A 16-bit 'short' value: 
short aShort = (short) 0b1010000101000101; 

// Some 32-bit 'int' values: 
int anInt1 = 0b10100001010001011010000101000101; 
int anInt2 = 0b101; 
int anInt3 = 0B101; // The B can be upper or lower case. 

// A 64-bit 'long' value. Note the "L" suffix:
long aLong = 0b1010000101000101101000010100010110100001010001011010000101000101L; 

// 二进制在数组等的使用 
final int[] phases = { 0b00110001, 0b01100010, 0b11000100, 0b10001001, 0b00010011, 0b00100110, 0b01001100, 0b10011000 };
```

##### 5.简化了可变参数方法的调用

   当程序员试图使用一个不可具体化的可变参数并调用一个*varargs* (可变)方法时，编辑器会生成一个“非安全操作”的警告。JDK 7将警告从call转移到了方法声明(methord declaration)的过程中。这样API设计者就可以使用vararg,因为警告的数量大大减少了。

##### 6.调用泛型类的构造方法时，可以省去泛型参数，编译器会自动判断。

JDK5中支持了泛型类型，我们需要在声明并赋值的时候，两侧都加上泛型类型。例如：

Map<String, String> map = new HashMap<String, String>();

在JDK7中，这种方式得以改进，现在你可以使用如下语句进行声明并赋值：

// 后面的"<>"可以不声明类型，PS：正常使用时尽量在new HashMap<>()括弧中声明初始大小

Map<String, String> map = new HashMap<>(); 

##### 7.Boolean类型反转，空指针安全,参与位运算

```java
Boolean Booleans.negate(Boolean booleanObj) 

True => False , False => True, Null => Null 

boolean Booleans.and(boolean[] array) 

boolean Booleans.or(boolean[] array) 

boolean Booleans.xor(boolean[] array) 

boolean Booleans.and(Boolean[] array) 

boolean Booleans.or(Boolean[] array) 

boolean Booleans.xor(Boolean[] array）
```

##### 8.char类型的equals方法

下面是JDK7中的比较方法：

```java
   booleanCharacter.equalsIgnoreCase(char ch1, char ch2)
```

在JDK8及以上，此方法已作废，JDK8常用的比较方法有两种，代码如下：

```java
char a = '5'; char b = '3'; 
// 第一种，使用 < > = != 运算符来判定，结果是：true 或者 false 
System.out.println(a > b); 

// 第二种，底层使用 a - b 的形式来实现，结果是：等于返回0，大于返回0以上，小于返回0以下 
System.out.println(Character.compare(a, b));

```

##### 9. 安全的加减乘除

```java
int Math.safeToInt(long value) 

int Math.safeNegate(int value) 

long Math.safeSubtract(long value1, int value2) 

long Math.safeSubtract(long value1, long value2) 

int Math.safeMultiply(int value1, int value2) 

long Math.safeMultiply(long value1, int value2) 

long Math.safeMultiply(long value1, long value2) 

long Math.safeNegate(long value) 

int Math.safeAdd(int value1, int value2) 

long Math.safeAdd(long value1, int value2) 

long Math.safeAdd(long value1, long value2) 

int Math.safeSubtract(int value1, int value2)
```

##### 10 .Map集合支持并发请求

注HashTable是线程安全的，Map是非线程安全的。但此处更新使得其也支持并发。另外，Map对象可这样定义：

```java
Map map = {name:"xxx",age:18};
```

11. ##### 新增一些取环境信息的工具方法

JDK7的工具方法如下：

```java
// IO临时文件夹 
File System.getJavaIoTempDir(); 

// JRE的安装目录
File System.getJavaHomeDir(); 

// 当前用户目录 
File System.getUserHomeDir(); 

// 启动Java进程时所在的目录
File System.getUserDir();
```

**PS：上面的几个方法在JDK8及以上版本都已经被作废，现在JDK8的新方法如下：**

```java
// 属性类 
Properties properties = System.getProperties(); 

// IO临时文件夹 
System.out.println(properties.getProperty("java.io.tmpdir")); 

// JRE的安装目录 
ystem.out.println(properties.getProperty("java.home"));

 // 当前用户目录 
System.out.println(properties.getProperty("user.home")); 

// 启动Java进程时所在的目录 
System.out.println(properties.getProperty("user.dir"));

如果想看到 Properties 中有多少的可显示属性，可使用以下代码来查询：
// 打印出所有的属性 
System.out.println(System.getProperties().toString());
```

##### 12.Try-with-resource语句

这个所谓的try-with-resources，是个语法糖。实际上就是自动调用资源的close()函数。和Python里的with语句差不多。 
例如：

```java
static String readFirstLineFromFile(String path) throws IOException { 
	try (BufferedReader br = new BufferedReader(new FileReader(path))) { 
		return br.readLine();
 		 }
 }
```

可以看到try语句多了个括号，而在括号里初始化了一个BufferedReader。这种在try后面加个括号，再初始化对象的语法就叫try-with-resources。 
实际上，相当于下面的代码（其实略有不同，下面会说明）：

```java
static String readFirstLineFromFileWithFinallyBlock(String path) throws IOException { 
	BufferedReader br = new BufferedReader(new FileReader(path));
 	try { 
		return br.readLine();
 	} finally { 
		if (br != null) br.close();
 		 } 
	} 
```

很容易可以猜想到，这是编绎器自动在try-with-resources后面增加了判断对象是否为null，如果不为null，则调用close()函数的的字节码。 
    只有实现了java.lang.AutoCloseable接口，或者java.io.Closable（实际上继随自java.lang.AutoCloseable）接口的对象，才会自动调用其close()函数。 
     有点不同的是java.io.Closable要求一实现者保证close函数可以被重复调用。而AutoCloseable的close()函数则不要求是幂等的。

##### 13.使用一个catch语言来处理多种异常类型

```java
public static void main (String[] args) throws Exception { 
	try { 
		testthrows(); 
		// 具体点在下面这个 | 符号，或者 
	} catch (IOException | SQLException ex) {
         throw ex; 
	} 
} 

public static void testthrows ()throws IOException, SQLException { 
}
```



### JDK1.8新特性

##### 1.接口的默认方法

接口中可以声明一个非抽象的方法做为默认的实现，但只能声明一个，且在方法的返回类型前要加上“default”关键字。

```java
interface Formula { 
	double calculate(int a); 
	default double sqrt(int a) { 
		return Math.sqrt(a); 
    } 
} 

Formula formula = new Formula() {  
	@Override 
	public double calculate(int a) { 
		return sqrt(a * 100); 
 	}; 
};
formula.calculate(100); // 100.0 
formula.sqrt(16); // 4.0
```

##### 2.lamda表达式

是对匿名比较器的简化，如：

```java
 Collections.sort(names,(String a, String b) -> {
       returnb.compareTo(a);
});
```

对于函数体只有一行代码的，你可以去掉大括号{}以及return关键字。如：

```java
 Collections.sort(names,(String a, String b) -> b.compareTo(a));

或：Collections.sort(names, (a, b) -> b.compareTo(a));

Collections.sort(names, (String a, Stringb) -> b.compareTo(a)); 
```

##### 3.函数式接口

指仅仅只包含一个抽象方法的接口，要加@FunctionalInterface注解

每一个lambda表达式都对应一个类型，通常是接口类型。而“函数式接口”是指仅仅只包含一个抽象方法的接口，每一个该类型的lambda表达式都会被匹配到这个抽象方法。因为默认方法不算抽象方法，所以你也可以给你的函数式接口添加默认方法。 
   我们可以将lambda表达式当作任意只包含一个抽象方法的接口类型，确保你的接口一定达到这个要求，只需要给你的接口添加@FunctionalInterface 注解，编译器如果发现你标注了这个注解的接口有多于一个抽象方法的时候会报错的。

```java
@FunctionalInterface 
interface Converter<F, T> { 
	Tconvert(F from); 
} 

Converter<String, Integer> converter= (from) -> 
Integer.valueOf(from); 
Integer converted =converter.convert("123"); 
System.out.println(converted); // 123
```

**4. 方法或者构造函数引用**

使用 :: 关键字来传递方法或者构造函 数引用

```java
Converter<String, Integer> converter= Integer::valueOf; 
Integer converted =converter.convert("123"); 
System.out.println(converted); // 123
```

Java 8 允许使用 :: 关键字来传递方法或者构造函数引用，上面的代码展示了如何引用一个静态方法，也可以引用一个对象的方法：

```java
converter = something::startsWith; 
String converted =converter.convert("Java"); 
System.out.println(converted); // "J"
```

看看构造函数是如何使用::关键字来引用的

```java
PersonFactory<Person> personFactory =Person::new;
Person person =personFactory.create("Peter", "Parker");
```

##### 5.多重Annotation 注解

Java 8允许我们把同一个类型的注解使用多次，只需要给该注解标注一下@Repeatable即可。

```java
@interface Hints { 
     int[] value(); 
} 

@Repeatable(Hints.class) 
@interface Hint { 
	tring value(); 
}
```

使用：

```java
@Hint("hint1") 
@Hint("hint2") 
lass Person {}
```

例子里java编译器会隐性的帮你定义好@Hints注解，了解这一点有助于你用反射来获取这些信息：

```java
Hint hint =Person.class.getAnnotation(Hint.class); 
System.out.println(hint); // null 
Hints hints1 = Person.class.getAnnotation(Hints.class); 
System.out.println(hints1.value().length); // 2 
Hint[] hints2 = Person.class.getAnnotationsByType(Hint.class); 
System.out.println(hints2.length); // 2
```

即便我们没有在Person类上定义@Hints注解，我们还是可以通过 getAnnotation(Hints.class)来获取@Hints注解，更加方便的方法是使用 getAnnotationsByType 可以直接获取到所有的@Hint注解。

另外Java 8的注解还增加到两种新的target上了：

```java
@Target({ElementType.TYPE_PARAMETER, ElementType.TYPE_USE}) 

@interface MyAnnotation {}
```

**6.访问接口的默认方法**

    例如，接口Formula定义了一个默认方法sqrt可以直接被formula的实例包括匿名对象访问到，但是在lambda表达式中这个是不行的。 
Lambda表达式中是无法访问到默认方法的，以下代码将无法编译：

```java
Formula formula = (a) -> sqrt( a * 100);
Built-in Functional Interfaces
```

JDK 1.8 API包含了很多内建的函数式接口，在老Java中常用到的比如Comparator或者Runnable接口，这些接口都增加了@FunctionalInterface注解以便能用在lambda上。 
   Java 8 API同样还提供了很多全新的函数式接口来让工作更加方便，有一些接口是来自Google Guava库里的，即便你对这些很熟悉了，还是有必要看看这些是如何扩展到lambda上使用的。

**Predicate接口** 
Predicate 接口只有一个参数，返回boolean类型。该接口包含多种默认方法来将Predicate组合成其他复杂的逻辑（比如：与，或，非）：

```java
Predicate<String> predicate =(s)->s.length>0;

predicate.test("foo");             // true

predicate.negate().test("foo");     // false

Predicate<Boolean> nonNull = Objects::nonNull;

Predicate<Boolean> isNull = Objects::isNull;

Predicate<String> isEmpty = String::isEmpty;

Predicate<String> isNotEmpty = isEmpty.negate();
```

**Function 接口** 
Function 接口有一个参数并且返回一个结果，并附带了一些可以和其他函数组合的默认方法（compose, andThen）：

```java
Function<String, Integer> toInteger =Integer::valueOf;

Function<String, String> backToString =toInteger.andThen(String::valueOf);

backToString.apply("123");    // "123"
```

**Supplier 接口** 
Supplier 接口返回一个任意范型的值，和Function接口不同的是该接口没有任何参数

```java
Supplier<Person> personSupplier = Person::new;

personSupplier.get();   // new Person
```

**Consumer 接口** 

Consumer 接口表示执行在单个参数上的操作。

```java
Consumer<Person> greeter = (p) ->System.out.println("Hello, " + p.firstName);

greeter.accept(new Person("Luke", "Skywalker"));
```

**Comparator接口** 
Comparator 是老Java中的经典接口， Java 8在此之上添加了多种默认方法

```java
Comparator<Person>comparator = (p1, p2) -> p1.firstName.compareTo(p2.firstName);

Person p1 = new Person("John","Doe");

Person p2 = new Person("Alice", "Wonderland");

comparator.compare(p1,p2);             //> 0

comparator.reversed().compare(p1, p2);  // < 0
```

**Optional 接口** 
Optional 不是函数是接口，这是个用来防止NullPointerException异常的辅助类型，这是下一届中将要用到的重要概念，现在先简单的看看这个接口能干什么： 
Optional 被定义为一个简单的容器，其值可能是null或者不是null。在Java 8之前一般某个函数应该返回非空对象但是偶尔却可能返回了null，而在Java 8中，不推荐你返回null而是返回Optional。代码如下:

```java
Optional<String> optional = Optional.of("bam");

optional.isPresent();          // true

optional.get();                // "bam"

optional.orElse("fallback");    // "bam"

optional.ifPresent((s) ->System.out.println(s.charAt(0)));     // "b"
```

**Stream 接口** 
java.util.Stream表示能应用在一组元素上一次执行的操作序列。Stream操作分为中间操作或者最终操作两种，最终操作返回一特定类型的计算结果，而中间操作返回Stream本身，这样你就可以将多个操作依次串起来。Stream的创建需要指定一个数据源，比如java.util.Collection的子类，List或者Set， Map不支持。Stream的操作可以串行执行或者并行执行。看看Stream是怎么用，首先创建实例代码的用到的数据List：

```java
List<String> stringCollection = newArrayList<>();

stringCollection.add("ddd2");

stringCollection.add("aaa2");

stringCollection.add("bbb1");
```

Java 8扩展了集合类，可以通过 Collection.stream() 或者 Collection.parallelStream() 来创建一个Stream。

**Filter过滤** 
过滤通过一个predicate接口来过滤并只保留符合条件的元素，该操作属于中间操作，所以我们可以在过滤后的结果来应用其他Stream操作（比如forEach）。forEach需要一个函数来对过滤后的元素依次执行。forEach是一个最终操作，所以我们不能在forEach之后来执行其他Stream操作。

```java
stringCollection.stream().filter((s) ->s.startsWith("a")).forEach(System.out::println); // "aaa2","aaa1"
```

**Sort排序** 
排序是一个中间操作，返回的是排序好后的Stream。如果你不指定一个自定义的Comparator则会使用默认排序。

```java
stringCollection.stream()
    .sorted()
    .filter((s) ->s.startsWith("a")).forEach(System.out::println); // "aaa1","aaa2"
```

需要注意的是，排序只创建了一个排列好后的Stream，而不会影响原有的数据源，排序之后原数据stringCollection是不会被修改的：

```java
System.out.println(stringCollection); // ddd2, aaa2,bbb1, aaa1, bbb3, ccc, bbb2, ddd1
```

**Map 映射** 
中间操作map会将元素根据指定的Function接口来依次将元素转成另外的对象，下面的示例展示了将字符串转换为大写字符串。你也可以通过map来讲对象转换成其他类型，map返回的Stream类型是根据你map传递进去的函数的返回值决定的。

```java
stringCollection

    .stream()

    .map(String::toUpperCase)

    .sorted((a, b) -> b.compareTo(a))

.forEach(System.out::println);// "DDD2", "DDD1", "CCC","BBB3", "BBB2", "AAA2", "AAA1"
```

Stream提供了多种匹配操作，允许检测指定的Predicate是否匹配整个Stream。所有的匹配操作都是最终操作，并返回一个boolean类型的值。

```java
boolean anyStartsWithA = stringCollection
        .stream()
        .anyMatch((s) ->s.startsWith("a"));

System.out.println(anyStartsWithA);     // true
boolean allStartsWithA = stringCollection
        .stream()
        .allMatch((s) ->s.startsWith("a"));
System.out.println(allStartsWithA);     // false
boolean noneStartsWithZ = stringCollection
        .stream()
        .noneMatch((s) ->s.startsWith("z"));
System.out.println(noneStartsWithZ);     // true
```

**Count计数** 
计数是一个最终操作，返回Stream中元素的个数，返回值类型是long。

```java
long startsWithB = stringCollection
        .stream()
        .filter((s) ->s.startsWith("b"))
        .count();
System.out.println(startsWithB);    // 3
```

这是一个最终操作，允许通过指定的函数来讲stream中的多个元素规约为一个元素，规越后的结果是通过Optional接口表示的：

```java
Optional<String>reduced = stringCollection
        .stream()
        .sorted()
        .reduce((s1, s2) -> s1 +"#" + s2);
reduced.ifPresent(System.out::println);  // "aaa1#aaa2#bbb1#bbb2#bbb3#ccc#ddd1#ddd2"
```

**并行Streams** 

前面提到过Stream有串行和并行两种，串行Stream上的操作是在一个线程中依次完成，而并行Stream则是在多个线程上同时执行。 
下面的例子展示了是如何通过并行Stream来提升性能： 
首先我们创建一个没有重复元素的大表：

```java
int max = 1000000;

List<String> values = new ArrayList<>(max);for (int i = 0; i < max; i++) {
    UUID uuid = UUID.randomUUID();
    values.add(uuid.toString());
}
```

然后我们计算一下排序这个Stream要耗时多久， 
**串行排序：**

```java
long t0 = System.nanoTime();long count = values.stream().sorted().count();

System.out.println(count);long t1 = System.nanoTime();long millis = TimeUnit.NANOSECONDS.toMillis(t1 - t0);

System.out.println(String.format("sequential sort took: %d ms",millis)); // 串行耗时: 899 ms
```

**并行排序：**

```java
long t0 = System.nanoTime();long count = values.parallelStream().sorted().count();

System.out.println(count);long t1 = System.nanoTime();long millis = TimeUnit.NANOSECONDS.toMillis(t1 - t0);

System.out.println(String.format("parallel sort took: %d ms", millis)); // 并行排序耗时: 472 ms
```

上面两个代码几乎是一样的，但是并行版的快了50%之多，唯一需要做的改动就是将stream()改为parallelStream()。

**Map** 
前面提到过，Map类型不支持stream，不过Map提供了一些新的有用的方法来处理一些日常任务。

```java
Map<Integer, String> map = new HashMap<>();for (int i = 0; i < 10; i++) {
    map.putIfAbsent(i, "val" + i);
}

map.forEach((id, val) ->System.out.println(val));
```

以上代码很容易理解， putIfAbsent 不需要我们做额外的存在性检查，而forEach则接收一个Consumer接口来对map里的每一个键值对进行操作。 
   下面的例子展示了map上的其他有用的函数：

```java
map.computeIfPresent(3, (num, val) -> val + num);
map.get(3);            // val33
map.computeIfPresent(9, (num, val) -> null);
map.containsKey(9);     // false
map.computeIfAbsent(23, num -> "val" + num);
map.containsKey(23);    // true
map.computeIfAbsent(3, num -> "bam");
map.get(3);            // val33
```

接下来展示如何在Map里删除一个键值全都匹配的项：

```java
map.remove(3, "val3");
map.get(3);            // val33
map.remove(3, "val33");
map.get(3);            // null
```

另外一个有用的方法：

```java
map.getOrDefault(42, "not found");  // notfound
```

对Map的元素做合并也变得很容易了：

```java
map.merge(9, "val9", (value, newValue) ->value.concat(newValue));
map.get(9);            // val9

map.merge(9, "concat", (value, newValue) ->value.concat(newValue));
map.get(9);            // val9concat
```

Merge做的事情是如果键名不存在则插入，否则则对原键对应的值做合并操作并重新插入到map中。

##### 7.lamda作用域

在lambda表达式中访问外层作用域和老版本的匿名对象中的方式很相似。 
可以直接访问标记了final的外层局部变量，或者实例的字段以及静态变量

##### 8.访问局部变量

可以直接在lambda表达式中访问外层的局部变量

```java
final int num = 1; 

Converter<Integer, String>stringConverter = 

(from) -> String.valueOf(from + num); 

stringConverter.convert(2);  // 3
```

但是和匿名对象不同的是，这里的变量num可以不用声明为final，该代码同样正确：

```java
int num = 1; 

Converter<Integer, String>stringConverter = 

(from) -> String.valueOf(from + num); 

stringConverter.convert(2);  // 3
```

不过这里的num必须不可被后面的代码修改（即隐性的具有final的语义），例如下面的就无法编译：

```java
int num = 1; 
Converter<Integer, String>stringConverter = 
(from) -> String.valueOf(from + num); 
num = 3;
```

##### 9.还增加了很多与函数式接口类似的接口以及与Map相关的API等……

  博客：<https://blog.csdn.net/wangnan537/article/details/49102877>

        <https://blog.csdn.net/aitangyong/article/details/54137067>

 




### jdk1.9新特性

##### 1、Java 平台级模块系统

当启动一个模块化应用时， JVM 会验证是否所有的模块都能使用，这基于 `requires` 语句——比脆弱的类路径迈进了一大步。模块允许你更好地强制结构化封装你的应用并明确依赖。

Java 9 的定义功能是一套全新的模块系统。当代码库越来越大，创建复杂，盘根错节的“意大利面条式代码”的几率呈指数级的增长。这时候就得面对两个基础的问题: 很难真正地对代码进行封装, 而系统并没有对不同部分（也就是 JAR 文件）之间的依赖关系有个明确的概念。每一个公共类都可以被类路径之下任何其它的公共类所访问到, 这样就会导致无意中使用了并不想被公开访问的 API。此外，类路径本身也存在问题: 你怎么知晓所有需要的 JAR 都已经有了, 或者是不是会有重复的项呢? 模块系统把这俩个问题都给解决了。

模块化的 JAR 文件都包含一个额外的模块描述器。在这个模块描述器中, 对其它模块的依赖是通过 “requires” 来表示的。另外, “exports” 语句控制着哪些包是可以被其它模块访问到的。所有不被导出的包默认都封装在模块的里面。如下是一个模块描述器的示例，存在于 “module-info.java” 文件中:

module blog { 

exports com.pluralsight.blog; 

requires cms; 

}

我们可以如下展示模块： 

 ![img](https://github.com/tianjiawei1/practice/blob/master/src/com/atguigu/image/W3VN.png)

请注意，两个模块都包含封装的包，因为它们没有被导出（使用橙色盾牌可视化）。 没有人会偶然地使用来自这些包中的类。Java 平台本身也使用自己的模块系统进行了模块化。通过封装 JDK 的内部类，平台更安全，持续改进也更容易。

当启动一个模块化应用时， JVM 会验证是否所有的模块都能使用，这基于 requires 语句——比脆弱的类路径迈进了一大步。模块允许你更好地强制结构化封装你的应用并明确依赖。你可以在这个课程中学习更多关于 Java 9 中模块工作的信息 

##### 2.Linking

当你使用具有显式依赖关系的模块和模块化的 JDK 时，新的可能性出现了。你的应用程序模块现在将声明其对其他应用程序模块的依赖以及对其所使用的 JDK 模块的依赖。为什么不使用这些信息创建一个最小的运行时环境，其中只包含运行应用程序所需的那些模块呢？ 这可以通过 Java 9 中的新的 jlink 工具实现。你可以创建针对应用程序进行优化的最小运行时映像而不需要使用完全加载 JDK 安装版本。

##### 3. JShell : 交互式 Java REPL

许多语言已经具有交互式编程环境，Java 现在加入了这个俱乐部。您可以从控制台启动 jshell ，并直接启动输入和执行 Java 代码。 jshell 的即时反馈使它成为探索 API 和尝试语言特性的好工具。

![img](https://github.com/tianjiawei1/practice/blob/master/src/com/atguigu/image/AYAX.png)

测试一个 Java 正则表达式是一个很好的说明 jshell 如何使您的生活更轻松的例子。 交互式 shell 还可以提供良好的教学环境以及提高生产力，您可以在此了解更多信息。在教人们如何编写 Java 的过程中，不再需要解释 “public static void main（String [] args）” 这句废话。

##### 4. 改进的 Javadoc

Javadoc 现在支持在 API 文档中的进行搜索。另外，Javadoc 的输出现在符合兼容 HTML5 标准。此外，你会注意到，每个 Javadoc 页面都包含有关 JDK 模块类或接口来源的信息。

![img](https://github.com/tianjiawei1/practice/blob/master/src/com/atguigu/image/J6FF.png)



##### 5.集合工厂方法

通常，您希望在代码中创建一个集合（例如，List 或 Set ），并直接用一些元素填充它。 实例化集合，几个 “add” 调用，使得代码重复。 Java 9，添加了几种集合工厂方法：

```jAVA
Set<Integer> ints = Set.of(1, 2, 3); 

List<String> strings = List.of("first", "second");
```

除了更短和更好阅读之外，这些方法也可以避免您选择特定的集合实现。 事实上，从工厂方法返回已放入数个元素的集合实现是高度优化的。这是可能的，因为它们是不可变的：在创建后，继续添加元素到这些集合会导致 “UnsupportedOperationException” 。

##### 6. 改进的 Stream API

长期以来，Stream API 都是 Java 标准库最好的改进之一。通过这套 API 可以在集合上建立用于转换的申明管道。在 Java 9 中它会变得更好。Stream 接口中添加了 4 个新的方法：dropWhile, takeWhile, ofNullable。还有个 iterate 方法的新重载方法，可以让你提供一个 Predicate (判断条件)来指定什么时候结束迭代：

```java
IntStream.iterate(1, i -> i < 100, i -> i + 1).forEach(System.out::println);
```

第二个参数是一个 Lambda，它会在当前 IntStream 中的元素到达 100 的时候返回 true。因此这个简单的示例是向控制台打印 1 到 99。

除了对 Stream 本身的扩展，Optional 和 Stream 之间的结合也得到了改进。现在可以通过 Optional 的新方法 `stram` 将一个 Optional 对象转换为一个(可能是空的) Stream 对象：

```java
Stream<Integer> s = Optional.of(1).stream();
```

在组合复杂的 Stream 管道时，将 Optional 转换为 Stream 非常有用。

##### 7. 私有接口方法

Java 8 为我们带来了接口的默认方法。 接口现在也可以包含行为，而不仅仅是方法签名。 但是，如果在接口上有几个默认方法，代码几乎相同，会发生什么情况？ 通常，您将重构这些方法，调用一个可复用的私有方法。 但默认方法不能是私有的。 将复用代码创建为一个默认方法不是一个解决方案，因为该辅助方法会成为公共API的一部分。 使用 Java 9，您可以向接口添加私有辅助方法来解决此问题：

```java
public interface MyInterface { 

void normalInterfaceMethod(); 

default void interfaceMethodWithDefault() { init(); } 

default void anotherDefaultMethod() { init(); }  // This method is not part of the public API exposed by MyInterface 

private void init() { System.out.println("Initializing"); } 

}
```

如果您使用默认方法开发 API ，那么私有接口方法可能有助于构建其实现。

##### 8. HTTP/2

Java 9 中有新的方式来处理 HTTP 调用。这个迟到的特性用于代替老旧的 `HttpURLConnection` API，并提供对 WebSocket 和 HTTP/2 的支持。注意：新的 HttpClient API 在 Java 9 中以所谓的孵化器模块交付。也就是说，这套 API 不能保证 100% 完成。不过你可以在 Java 9 中开始使用这套 API：

```java
HttpClient client = HttpClient.newHttpClient(); 

HttpRequest req = 
HttpRequest.newBuilder(URI.create("http://www.google.com")) 
	.header("User-Agent","Java") 
	.GET() 
	.build(); 

HttpResponse<String> resp = client.send(req, HttpResponse.BodyHandler.asString());
HttpResponse<String> resp = client.send(req, HttpResponse.BodyHandler.asString());
```


除了这个简单的请求/响应模型之外，HttpClient 还提供了新的 API 来处理 HTTP/2 的特性，比如流和服务端推送。

##### 9. 多版本兼容 JAR

我们最后要来着重介绍的这个特性对于库的维护者而言是个特别好的消息。当一个新版本的 Java 出现的时候，你的库用户要花费数年时间才会切换到这个新的版本。这就意味着库得去向后兼容你想要支持的最老的 Java 版本 (许多情况下就是 Java 6 或者 7)。这实际上意味着未来的很长一段时间，你都不能在库中运用 Java 9 所提供的新特性。幸运的是，多版本兼容 JAR 功能能让你创建仅在特定版本的 Java 环境中运行库程序时选择使用的 class 版本：

```txt
multirelease.jar 

├── META-INF 
│    └── versions 
│         └── 9 
│             └── multirelease 
│                   └── Helper.class 
├── multirelease 
├── Helper.class 
└── Main.class
```

在上述场景中， multirelease.jar 可以在 Java 9 中使用, 不过 Helper 这个类使用的不是顶层的 multirelease.Helper 这个 class, 而是处在“META-INF/versions/9”下面的这个。这是特别为 Java 9 准备的 class 版本，可以运用 Java 9 所提供的特性和库。同时，在早期的 Java 诸版本中使用这个 JAR 也是能运行的，因为较老版本的 Java 只会看到顶层的这个 Helper 类。

##### 10.更多

###### 10.1简化了的进程API

目前，Java控制与管理系统进程的能力是有限的,为了获得操作系统的一些信息需要调用本地程序或者其他变通方案。然而，在Java 9中将会新增一些新的、直接明了的方法来处理进程ID、名字和状态以及枚举多个JVM和进程等，从而扩展Java与操作系统的交互能力。更多相关信息参见JEP102。

###### 10.2轻量级的JSON API

尽管目前有多种处理JSON的Java工具（如Google的Gson、阿里巴巴的FastJson、IBM的Json4J等），但JSON API是Java语言的一部分，轻量并且运用了Java 8的新特性。JSON API将放在java.util包里一起发布，这样，开发者就可以直接使用JDK而无需再引入第三方JSON工具包了。更多相关信息参见JEP198。

###### 10.3钱和货币的相关API

Java 9引入了新的货币API, 用来表示货币, 并支持币种之间的转换和各种复杂运算。更多的相关具体信息,参见JavaMoney项目和JSR354。

###### 10.4改善锁争用机制

  锁争用限制了许多Java多线程应用性能，新的锁争用机制改善了Java对象监视器的性能，并得到了多种基准测试的验证（如Volano）,这类测试可以估算JVM的极限吞吐量。实际中, 新的锁争用机制在22种不同的基准测试中都得到了出色的成绩。如果新的机制能在Java 9中得到应用的话, 应用程序的性能将会大大提升。更多相关信息参见JEP143。

###### 10.5代码分段缓存

Java 9的另一个性能提升来自于JIT(Just-in-time)编译器。当某段代码被大量重复执行的时候, 虚拟机会把这段代码编译成机器码(native code)并储存在代码缓存里面, 继而通过访问缓存中不同分段的代码来提升编译器的效率。代码分段缓存机制将会提升许多方面的性能，如当JVM进行垃圾回收扫描的时候，就可以直接跳过永驻代码,从而提升效率。更多相关信息参见JEP197。

###### 10.6智能Java编译工具

智能Java编译工具（sjavac）的第一阶段始于JEP139这个项目, 用于在多核处理器情况下提升JDK的编译速度。如今，这个项目已经进入第二阶段即JEP199, 其目的是改进Java编译工具，并取代目前JDK编译工具javac，继而成为Java环境默认的通用的智能编译工具。更多相关信息参见JEP199。

**更多新特性可以访问：**[**http://openjdk.java.net/projects/jdk9/**](http://openjdk.java.net/projects/jdk9/) **来查看。**

 
