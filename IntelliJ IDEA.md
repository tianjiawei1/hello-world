## 一、IntelliJ IDEA介绍--EclipseIBM

## 1.JetBrains公司介绍

	IDEA(https://www.jetbrains.com/idea/)是JetBrains公司的产品，公司旗下还有其它产品，比如：
	
		WebStorm：用于开发JavaScript、HTML5、CSS3等前端技术；

 		PyCharm：用于开发python

 		PhpStorm：用于开发PHP

		RubyMine：用于开发Ruby/Rails
	
		AppCode：用于开发Objective -C/Swift
	
		CLion：用于开发C/C++
	
		DataGrip：用于开发数据库和
	
		SQL Rider：用于开发.NET
	
		 GoLand：用于开发Go
	
		Android Studio：用于开发android(google基于IDEA社区版进行迭代)

![1535478019789](C:\Users\ADMINI~1\AppData\Local\Temp\1535478019789.png)

## 2. IntelliJ IDEA介绍

      	IDEA，全称IntelliJ IDEA，是Java语言的集成开发环境，IDEA在业界被公认为是最好的java开发工具之一，尤其在智能代码助手、代码自动提示、重构、J2EE支持、Ant、JUnit、CVS整合、代码审查、创新的GUI设计等方面的功能可以说是超常的。
    
    	IntelliJ IDEA 在2015年的官网上这样介绍自己：

Excel at enterprise, mobile and web development with Java, Scala and Groovy, with all the latest modern technologies and frameworks available out of the box.

	简明翻译：IntelliJ IDEA 主要用于支持Java、Scala、Groovy 等语言的开发工具，同时具备支持目前主流的技术和框架，擅长于企业应用、移动应用和Web 应用的开发。

##  3.IDE的主要功能介绍

语言支持上：

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml\wpsAFA.tmp.jpg)

其他支持：

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml\wpsA41E.tmp.jpg)



## 4.IDEA**的主要优势：**(相较于Eclipse**而言**)

	①强大的整合能力。比如：Git、Maven、Spring等
	
	②提示功能的快速、便捷
	
	③提示功能的范围广
	
	④好用的快捷键和代码模板private static final psf
	
	⑤精准搜索

## 5.IDEA**的下载地址：**(**官网**)

https://www.jetbrains.com/idea/download/#section=windows

IDEA分为两个版本：旗舰版**(Ultimate)**和社区版**(Community)**。

旗舰版收费(限30天免费试用)，社区版免费，这和Eclipse有很大区别。

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml\wps5535.tmp.jpg)

## 6. 官网提供的详细使用文档：

https://www.jetbrains.com/help/idea/meet-intellij-idea.html

# 二、**windows**下安装过程

## 1.具体安装过程 

######   		百度

##  2. 查看安装目录结构

![1535469135806](C:\Users\ADMINI~1\AppData\Local\Temp\1535469135806.png)

##### bin：容器，执行文件和启动参数等

##### help：快捷键文档和其他帮助文档

##### jre64：64 位java 运行环境

##### lib：idea 依赖的类库

##### license：各个插件许可

##### plugin：插件

其中 binbin 目录下： 

![1535469226224](C:\Users\ADMINI~1\AppData\Local\Temp\1535469226224.png)

如何调整 VM 配 置文件：
![1535469328440](C:\Users\ADMINI~1\AppData\Local\Temp\1535469328440.png)

1. 根据电脑系统的位数，选择 32 位的 VM 配置文件或者 64 位的 VM 配置文件
2. 32 位操作系统内存不会超过位操作系统内存不会超过 4G，所以没有多大空间可调整建议不用了
3. 64 位操作系统中 8G 内存以下的机子或是静态页面开发者无需修改内存以下的机子或是静态页面开发者无需修改。
4. 64 位操作系统且内存大于 位操作系统且内存大于 8G 的，如果你是开发大型项目、 Java Java Java Java 项目或是 Android Android Android Android Android 项目 ，建议进行修改，常的就是下面 3 个参数 ：

| -Xms128m，16 G 内存的机器可尝试设置为 -Xms512m<br/>(设置初始的内存数，增加该值可 以提高 Java Java 程序的启动速度。 ) -Xmx750m，16 G 内存的机器可尝试设置为 -Xmx1500m
(设置最大内存 数，提高该值可以减少Garage Garage Garage收集的频率，提高程序性能 )
-XX:ReservedCodeCacheSize=240m，16G 内存的机器可尝试设置为 -XX:ReservedCodeCacheSize=500m

(保留代码占用的内存容量 ) |
| ------------------------------------------------------------ |
|                                                              |

## 3. 查看设置目录结构 

![1535469468208](C:\Users\ADMINI~1\AppData\Local\Temp\1535469468208.png)

这是IDEA的各种配置的保存目录。这个设置目录有一个特性，就是你删除掉整个目录之后，重新启动IntelliJ IDEA 会再自动帮你生成一个全新的默认配置，所以很多时候如果你把IntelliJ IDEA 配置改坏了，没关系，删掉该目录，一切都会还原到默认。

### 3.1 config目录
config目录是 IntelliJ IDEA 个性化化配置目录，或者说是整个 IDE 设置目录。此目录可看成是最重要的目录，没有之一，如果你还记得安装篇的介绍的时候，安装新版本的 IntelliJ IDEA 会自动扫描硬盘上的旧配置目录，指的就是该目录。这个目录主要记录了：IDE 主要配置功能、自定义的代码模板、自定义的文件模板、自定义的快捷键、Project 的 tasks 记录等等个性化的设置。 比如： 

![1535469696532](C:\Users\ADMINI~1\AppData\Local\Temp\1535469696532.png)

###  3.2**system**目录

system目录是IntelliJ IDEA 系统文件目录，是IntelliJ IDEA 与开发项目一个桥梁目录，里面主要有：缓存、索引、容器文件输出等等，虽然不是最重要目录，但也是最不可或缺的目录之一。比如：

![1535469755362](C:\Users\ADMINI~1\AppData\Local\Temp\1535469755362.png)

## 4.设置插件

![1535469807746](C:\Users\ADMINI~1\AppData\Local\Temp\1535469807746.png)

![1535469826171](C:\Users\ADMINI~1\AppData\Local\Temp\1535469826171.png)

设置IDEA中的各种插件，可以选择自定义设置、删除，或者安装本身不存在的插件（比如：支持Scala的插件）。这里不设置，后面也可以通过界面菜单栏的settings进行设置。

IDEA插件官方下载地址：https://plugins.jetbrains.com/idea

# 三、创建 Java工程 

## 1.创建 Java工程

![1535470101403](C:\Users\ADMINI~1\AppData\Local\Temp\1535470101403.png)

Create New Project:创建一个新的工程

Import Project:导入一个现有的工程

Open:打开一个已有工程。比如：可以打开Eclipse项目

Check out from Version Control:可以通过服务器上的项目地址check out Github上面项目或其他Git托管服务器上的项目

这里选择Create New Project，需要明确一下概念：

**IntelliJ IDEA 没有类似Eclipse 的工作空间的概念（Workspaces），最大单元就是Project。这里可以把Project理解为Eclipse中的Workspace。**

![1535470455746](C:\Users\ADMINI~1\AppData\Local\Temp\1535470455746.png)

选择指定目录下的JDK作为Project SDK。

如果要创建Web工程，则需要勾选上面的Web Application。如果不需要创建Web工程的话，则不需要勾选。这里先不勾选，只是创建简单的Java工程。

其中， 选择 New：
选择  jdk的安装路径所在位置： 

![1535470516525](C:\Users\ADMINI~1\AppData\Local\Temp\1535470516525.png)

点击 OK 以后，选择 Next:

![1535470551540](C:\Users\ADMINI~1\AppData\Local\Temp\1535470551540.png)

这里不用勾选。选择Next，进入下一个页面：

![1535470581068](C:\Users\ADMINI~1\AppData\Local\Temp\1535470581068.png)

给创建的工程起一个名字，点击finish。

![1535470600574](C:\Users\ADMINI~1\AppData\Local\Temp\1535470600574.png)

点击 OK 即可。

## 2.设置显示常见的视图 

![1535470643999](C:\Users\ADMINI~1\AppData\Local\Temp\1535470643999.png)

调出工具条和按钮组

## 3.工程界面展示

![1535470683508](C:\Users\ADMINI~1\AppData\Local\Temp\1535470683508.png)

工程下的src类似于Eclipse下的src目录，用于存放代码。

工程下的.idea和project01.iml文件都是IDEA工程特有的。类似于Eclipse工程下的.settings、.classpath、.project等。

## **4.**创建**package**和**class**

![1535470790605](C:\Users\ADMINI~1\AppData\Local\Temp\1535470790605.png)

![1535470798048](C:\Users\ADMINI~1\AppData\Local\Temp\1535470798048.png)

在包下 new-class：

![1535470823273](C:\Users\ADMINI~1\AppData\Local\Temp\1535470823273.png)

![1535470829277](C:\Users\ADMINI~1\AppData\Local\Temp\1535470829277.png)

不管是创建class，还是interface，还是annotation，都是选择new–java class，

然后在下拉框中选择创建的结构的类型。

接着在类HelloWorld里声明主方法，输出helloworld，完成测试

![1535470876795](C:\Users\ADMINI~1\AppData\Local\Temp\1535470876795.png)

 ## 5.创建模块 (Module) 

**5.1在Eclipse中我们有Workspace（工作空间）和Project（工程）的概念，在IDEA中只有Project（工程）和Module（模块）的概念。这里的对应关系为：**

**Eclipse**中**workspace**   相当于**IDEA**中的**Project**

**Eclipse**中**Project**           相当于**IDEA**中的**Module**

**5.2从Eclipse 转过来的人总是下意识地要在同一个窗口管理n 个项目，这在IntelliJ IDEA 是无法做到的。IntelliJ IDEA 提供的解决方案是打开多个项目实例，即打开多个项目窗口。即：一个Project 打开一个Window 窗口。**

**5.3在IntelliJ IDEA 中Project 是最顶级的级别，次级别是Module。一个Project可以有多个Module。目前主流的大型项目都是分布式部署的，结构都是类似这种多Module 结构。**

![1535471218708](C:\Users\ADMINI~1\AppData\Local\Temp\1535471218708.png)

这类项目一般是这样划分的，比如：core Module、web Module、plugin Module、solr Module 等等，模块之间彼此可以相互依赖。通过这些Module 的命名也可以看出，他们之间都是处于同一个项目业务下的模块，彼此之间是有不可分割的业务关系的。举例：

![1535471245106](C:\Users\ADMINI~1\AppData\Local\Temp\1535471245106.png)

相比较于多Module 项目，小项目就无需搞得这么复杂。只有一个Module 的结构IntelliJ IDEA 也是支持的，并且IntelliJ IDEA 创建项目的时候，默认就是单Module 的结构的。

下面，我们演示如何创建Module:

![1535471286116](C:\Users\ADMINI~1\AppData\Local\Temp\1535471286116.png)

![1535471293683](C:\Users\ADMINI~1\AppData\Local\Temp\1535471293683.png)

接着选择Next:

![1535471318862](C:\Users\ADMINI~1\AppData\Local\Temp\1535471318862.png)

之后，我们可以在Module的src里写代码，此时Project工程下的src就没什么用了。可以删掉。

## 6. 如何删除模块

![1535471374375](C:\Users\ADMINI~1\AppData\Local\Temp\1535471374375.png)

![1535471381729](C:\Users\ADMINI~1\AppData\Local\Temp\1535471381729.png)

![1535471386437](C:\Users\ADMINI~1\AppData\Local\Temp\1535471386437.png)

![1535471395636](C:\Users\ADMINI~1\AppData\Local\Temp\1535471395636.png)

此时的删除，会从硬盘上将此module删除掉。

##  7.查看项目配置

![1535471433939](C:\Users\ADMINI~1\AppData\Local\Temp\1535471433939.png)

进入项目结构：

![1535471449934](C:\Users\ADMINI~1\AppData\Local\Temp\1535471449934.png)

# 四、常用配置

## 1.设置鼠标悬浮提示

![1535471548112](C:\Users\ADMINI~1\AppData\Local\Temp\1535471548112.png)

## 2.设置自动导包功能

![1535471577766](C:\Users\ADMINI~1\AppData\Local\Temp\1535471577766.png)

Add unambiguous imports on the fly：自动导入不明确的结构

Optimize imports on the fly：自动帮我们优化导入的包

## 3.忽略大小写提示

![1535471758976](C:\Users\ADMINI~1\AppData\Local\Temp\1535471758976.png)

IntelliJ IDEA 的代码提示和补充功能有一个特性：区分大小写。如上图标注所示，默认就是First letter 区分大小写的。

区分大小写的情况是这样的：比如我们在Java 代码文件中输入stringBuffer，IntelliJ IDEA 默认是不会帮我们提示或是代码补充的，但是如果我们输入StringBuffer 就可以进行代码提示和补充

如果想不区分大小写的话，改为None 选项即可

## 4. Editor –File and Code Templates

**修改类头的文档注释信息** 

![1535471898365](C:\Users\ADMINI~1\AppData\Local\Temp\1535471898365.png)

/** 

@author shkstart 

@create **${YEAR}**-**${MONTH}**-**${DAY} ${TIME}**

 */

常用的预设的变量，这里直接贴出官网给的：

*   ${PACKAGE_NAME} -the name of the target package where the new class or interface will be created. 

* ${PROJECT_NAME} -the name of the current project. 

* ${FILE_NAME} -the name of the PHP file that will be created. 

* ${NAME} -the name of the new file which you specify in the New File dialog box during the file creation. 

* ${USER} -the login name of the current user. 

* ${DATE} -the current system date. 

* ${TIME} -the current system time. 

* ${YEAR} -the current year. 

* ${MONTH} -the current month. 

* ${DAY} -the current day of the month. 

* ${HOUR} -the current hour. 

* ${MINUTE} -the current minute. 

* ${PRODUCT_NAME} -the name of the IDE in which the file will be created. 

* ${MONTH_NAME_SHORT} -the first 3 letters of the month name. Example: Jan, Feb, etc. 

* ${MONTH_NAME_FULL} -full name of a month. Example: January, February, etc

  ##  5.Editor –File Encodings

  #### 5.1设置项目文件编码

![1535472204542](C:\Users\ADMINI~1\AppData\Local\Temp\1535472204542.png)

说明：Transparent native-to-ascii conversion 主要用于转换ascii，一般都要勾选，不然Properties 文件中的注释显示的都不会是中文。

#### 5.2设置当前源文件的编码

![1535472253917](C:\Users\ADMINI~1\AppData\Local\Temp\1535472253917.png)

![1535472259600](C:\Users\ADMINI~1\AppData\Local\Temp\1535472259600.png)

对单独文件的编码修改还可以点击右下角的编码设置区。如果代码内容中包含中文，则会弹出如上的操作选择。其中：

①Reload 表示使用新编码重新加载，新编码不会保存到文件中，重新打开此文件，旧编码是什么依旧还是什么。

②Convert 表示使用新编码进行转换，新编码会保存到文件中，重新打开此文件，新编码是什么则是什么。

③含有中文的代码文件，Convert 之后可能会使中文变成乱码，所以在转换成请做好备份，不然可能出现转换过程变成乱码，无法还原。

## 6.Build,Execution,Deployment

**设置自动编译**

![1535472340707](C:\Users\ADMINI~1\AppData\Local\Temp\1535472340707.png)

**构建就是以我们编写的java代码、框架配置文件、国际化等其他资源文件、JSP页面和图片等资源作为“原材料”，去“生产”出一个可以运行的项目的 **

Intellij Idea默认状态为不自动编译状态，Eclipse默认为自动编译：

![1535472411289](C:\Users\ADMINI~1\AppData\Local\Temp\1535472411289.png)



很多朋友都是从Eclipse转到Intellij的，这常常导致我们在需要操作class文件时忘记对修改后的java类文件进行重新编译，从而对旧文件进行了操作。

## 7设置快捷键 (Keymap) 

### 7.1设置快捷为**Eclipse**的快捷键

![1535472794695](C:\Users\ADMINI~1\AppData\Local\Temp\1535472794695.png)

### 7.2通过快捷键功能修改快捷键设置

![1535472843104](C:\Users\ADMINI~1\AppData\Local\Temp\1535472843104.png)

### 7.3通过指定快捷键，查看或修改其功能

![1535472874583](C:\Users\ADMINI~1\AppData\Local\Temp\1535472874583.png)

### 7.4导入已有的设置

![1535472925894](C:\Users\ADMINI~1\AppData\Local\Temp\1535472925894.png)

![1535472934635](C:\Users\ADMINI~1\AppData\Local\Temp\1535472934635.png)

点击0K之后，重启IDEA即可。

# 五、关于模板**(Templates)**

(Editor –Live Templates和Editor –General –Postfix Completion)

###  1.LiveTemplates(**实时代码模板**)功能介绍

它的原理就是配置一些常用代码字母缩写，在输入简写时可以出现你预定义的固定模式的代码，使得开发效率大大提高，同时也可以增加个性化。最简单的例子就是在Java中输入sout会出现System.out.println();

### 2.已有的常用模板

**Postfix Completion**默认如下：

![1535473985034](C:\Users\ADMINI~1\AppData\Local\Temp\1535473985034.png)

**Live Templates 默认如下**

![1535474020379](C:\Users\ADMINI~1\AppData\Local\Temp\1535474020379.png)

**二者的区别：Live Templates 可以自定义，而Postfix Completion不可以。同时，有些操作二者都提供了模板，Postfix Templates较Live Templates能快0.01秒**

举例：

**2.1 psvm :** 可生成**main**方法

**2.2 sout : System.out.println()** 快捷输出

类似的：

soutp=System.out.println("方法形参名= "+ 形参名);

soutv=System.out.println("变量名= " + 变量);

soutm=System.out.println("当前类名.当前方法");

“abc”.sout  => System.out.println("abc");

**2.3fori :** 可生成**for**循环

类似的：

iter：可生成增强for循环

itar：可生成普通for循环

**2.4 list.for :** 可生成集合**list**的**for**循环

List<String> list = new ArrayList<String>();

输入: list.for 即可输出

for(String s:list){

}

又如：list.fori  或list.forr

**2.5 ifn**：可生成**if(xxx = null)**

类似的：

inn：可生成if(xxx != null)或xxx.nn或xxx.null

**2.6 prsf**：可生成**private static final** 

类似的：

psf：可生成public static final

psfi：可生成public static final int

psfs：可生成public static final String

### 3.修改现有模板**:Live Templates**

如果对于现有的模板，感觉不习惯、不适应的，可以修改：

修改**1**：

通过调用psvm调用main方法不习惯，可以改为跟Eclipse一样，使用main调取。

![1535474180681](C:\Users\ADMINI~1\AppData\Local\Temp\1535474180681.png)

修改**2**：![1535474203790](C:\Users\ADMINI~1\AppData\Local\Temp\1535474203790.png)

类似的还可以修改psfs。

### 4.自定义模板

IDEA提供了很多现成的Templates。但你也可以根据自己的需要创建新的Template。

![1535474271681](C:\Users\ADMINI~1\AppData\Local\Temp\1535474271681.png)

先定义一个模板的组：

 ![1535474285576](C:\Users\ADMINI~1\AppData\Local\Temp\1535474285576.png)

![1535474308226](C:\Users\ADMINI~1\AppData\Local\Temp\1535474308226.png)

选中自定义的模板组，点击”+”来定义模板。

![1535474329323](C:\Users\ADMINI~1\AppData\Local\Temp\1535474329323.png)‘

1. Abbreviation:模板的缩略名称

2. Description:模板的描述

3. Template text:模板的代码片段

4. 应用范围。比如点击Define。选择如下：

![1535474364910](C:\Users\ADMINI~1\AppData\Local\Temp\1535474364910.png)

可以如上的方式定义个测试方法，然后在java类文件中测试即可。

类似的可以再配置如下的几个Template:

1.![1535474410078](C:\Users\ADMINI~1\AppData\Local\Temp\1535474410078.png)

2.![1535474418194](C:\Users\ADMINI~1\AppData\Local\Temp\1535474418194.png)

# 六、创建**JavaWebProject** 或**Module**

### 1.创建的静态**Java Web**

![1535474510035](C:\Users\ADMINI~1\AppData\Local\Temp\1535474510035.png)

![1535474518933](C:\Users\ADMINI~1\AppData\Local\Temp\1535474518933.png)

### 2. 创建动态的**Java Web**

**2.1**创建动态**Web**的**module**

工程栏空白处new –module:

![1535474574458](C:\Users\ADMINI~1\AppData\Local\Temp\1535474574458.png)

![1535474583799](C:\Users\ADMINI~1\AppData\Local\Temp\1535474583799.png)

这里一定要勾选Web Application，才能创建一个Web工程。

![1535474605075](C:\Users\ADMINI~1\AppData\Local\Temp\1535474605075.png)

提供Web工程名。这里注意修改一下Content root和Module file location。

创建以后的工程结构如下：

![1535474648199](C:\Users\ADMINI~1\AppData\Local\Temp\1535474648199.png)

打开index.jsp。修改为如下内容。这里你会发现IDEA的代码提示功能要强于Eclipse.

<%@ **page contentType**="**text/html;charset=UTF-8**" **language**="**java**" %>

 <**html**> 

	<**head**> 
	
		<**title**>尚硅谷官网</**title**> 
	
	</**head**>

 	<**body**> 

		<**h1 style="color**: **red"**>hello IDEA!</**h1**> 
	
	</**body**>

 </**html**>

**2.2**配置**Tomcat**

注意事项：

显示运行以后的Tomcat的信息：

![1535474803627](C:\Users\ADMINI~1\AppData\Local\Temp\1535474803627.png)

可以点击红框，刚点击完毕并不能马上关闭服务器，只是断开了与服务器的连接，稍后当停止按钮显示为灰色，才表示关闭。

# 七、关联数据库

### 1.关联方式

![1535474890714](C:\Users\ADMINI~1\AppData\Local\Temp\1535474890714.png)

![1535474898152](C:\Users\ADMINI~1\AppData\Local\Temp\1535474898152.png)



![1535474905456](C:\Users\ADMINI~1\AppData\Local\Temp\1535474905456.png)

表面上很多人认为配置Database 就是为了有一个GUI 管理数据库功能，但是这并不是IntelliJ IDEA 的Database 最重要特性。数据库的GUI 工具有很多，IntelliJ IDEA 的Database 也没有太明显的优势。IntelliJ IDEA 的Database 最大特性就是对于Java Web 项目来讲，常使用的ORM 框架，如Hibernate、Mybatis 有很好的支持，比如配置好了Database 之后，IntelliJ IDEA 会自动识别domain 对象与数据表的关系，也可以通过Database 的数据表直接生成domain 对象等等。

### 2.常用操作

![1535474954227](C:\Users\ADMINI~1\AppData\Local\Temp\1535474954227.png)

图标1：同步当前的数据库连接。这个是最重要的操作。配置好连接以后或通过其他工具操作数据库以后，需要及时同步。

图标2：配置当前的连接

图标3：断开当前的连接。

图标4：显示相应数据库对象的数据

图标5：编辑修改当前数据库对象

# 八、版本控制**(Version Control)**

![1535475257581](C:\Users\ADMINI~1\AppData\Local\Temp\1535475257581.png)

很多人认为IntelliJ IDEA 自带了SVN 或是Git 等版本控制工具，认为只要安装了IntelliJ IDEA 就可以完全使用版本控制应有的功能。这完全是一种错误的解读，IntelliJ IDEA 是自带对这些版本控制工具的插件支持，但是该装什么版本控制客户端还是要照样装的。

![1535475281616](C:\Users\ADMINI~1\AppData\Local\Temp\1535475281616.png)

IntelliJ IDEA 对版本控制的支持是以插件化的方式来实现的。旗舰版默认支持目前主流的版本控制软件：CVS、Subversion（SVN）、Git、Mercurial、Perforce、TFS。又因为目前太多人使用Github 进行协同或是项目版本管理，所以IntelliJ IDEA 同时自带了Github 插件，方便Checkout 和管理你的Github 项目。

在实际开发中，发现在IDEA中使用SVN的经历不算愉快，经常会遇到很多问题，比如紧急情况下IDEA无法更新、提交等。所以这里，谈下在IDEA中使用Git。

### 1. 提前安装好**Git**的客户端

### 2.关联git.exe

![1535475391069](C:\Users\ADMINI~1\AppData\Local\Temp\1535475391069.png)

### 3.关联**GitHub**上的账户，并测试连接

![1535475421757](C:\Users\ADMINI~1\AppData\Local\Temp\1535475421757.png)

 ### 4.在**GitHub**上创建账户下的一个新的仓库作为测试：

![1535475454868](C:\Users\ADMINI~1\AppData\Local\Temp\1535475454868.png)

![1535475460666](C:\Users\ADMINI~1\AppData\Local\Temp\1535475460666.png)

### 5.支持从当前登录的**Github**账号上直接**Checkout**项目

![1535475487853](C:\Users\ADMINI~1\AppData\Local\Temp\1535475487853.png)

### 6.在**IDEA**中**cloneGitHub**上的仓库：

![1535475514548](C:\Users\ADMINI~1\AppData\Local\Temp\1535475514548.png)

这里需要在GitHub的自己的账户下，复制项目仓库路径，填写到上图GitRepository URL中。如下：

![1535475533141](C:\Users\ADMINI~1\AppData\Local\Temp\1535475533141.png)

### 7.连接成功以后，会下载**github**上的项目

 ![1535475567912](C:\Users\ADMINI~1\AppData\Local\Temp\1535475567912.png)

![1535475573165](C:\Users\ADMINI~1\AppData\Local\Temp\1535475573165.png)

![1535475579671](C:\Users\ADMINI~1\AppData\Local\Temp\1535475579671.png)

### 8.除此之外，还可以通过如下的方式连接**GitHub**

![1535475607719](C:\Users\ADMINI~1\AppData\Local\Temp\1535475607719.png)

### 9.本地代码分享到**GitHub**

![1535475637237](C:\Users\ADMINI~1\AppData\Local\Temp\1535475637237.png)![1535475651372](C:\Users\ADMINI~1\AppData\Local\Temp\1535475651372.png)

此时会在GitHub上创建一个新的仓库，而非更新已经存在的仓库。

### 10.**Git**的常用操作

![1535475693339](C:\Users\ADMINI~1\AppData\Local\Temp\1535475693339.png)

clone：拷贝远程仓库
commit ：本地提交
push ：远程提交
pull ：更新到本地

### 11.没有使用**Git**时本地历史记录的查看

![1535475787264](C:\Users\ADMINI~1\AppData\Local\Temp\1535475787264.png)



![1535475794246](C:\Users\ADMINI~1\AppData\Local\Temp\1535475794246.png)

即使我们项目没有使用版本控制功能，IntelliJ IDEA 也给我们提供了本地文件历史记录。

# 九、断点调试

### 1.Debug的设置

![1535475944865](C:\Users\ADMINI~1\AppData\Local\Temp\1535475944865.png)

设置Debug 连接方式，默认是Socket。Shared memory 是Windows 特有的一个属性，一般在Windows 系统下建议使用此设置，内存占用相对较少。

### 2.常用断点调试快捷键

![1535476033565](C:\Users\ADMINI~1\AppData\Local\Temp\1535476033565.png)

### 3.条件断点

说明：

调试的时候，在循环里增加条件判断，可以极大的提高效率，心情也能愉悦。

具体操作：

在断点处右击调出条件断点。可以在满足某个条件下，实施断点。

查看表达式的值**(Ctrl + u)**：

选择行，ctrl + u。还可以在查看框中输入编写代码时的其他方法：

![1535476080162](C:\Users\ADMINI~1\AppData\Local\Temp\1535476080162.png)

# 十、配置Maven

### 1.Maven的介绍

Make -> Ant ->Maven -> Gradle

Maven是Apache提供的一款自动化构建工具，用于自动化构建和依赖管理。开发团队基本不用花多少时间就能自动完成工程的基础构建配置，因为Maven使用了一个标准的目录结构和一个默认的构建生命周期。在如下环节中，Maven使得开发者工作变得更简单。

构建环节：

![1535476177094](C:\Users\ADMINI~1\AppData\Local\Temp\1535476177094.png)

* 清理：表示在编译代码前将之前生成的内容删除 

*  编译：将源代码编译为字节码 

*  测试：运行单元测试用例程序 

* 报告：测试程序的结果 

* 打包：将java项目打成jar包；将Web项目打成war包 

* 安装：将jar或war生成到Maven仓库中 

* 部署：将jar或war从Maven仓库中部署到Web服务器上运行 

### **2. Maven**的配置

![1535476434246](C:\Users\ADMINI~1\AppData\Local\Temp\1535476434246.png)

Mavenhome directory：可以指定本地Maven 的安装目录所在，因为我已经配置了M2_HOME 系统参数，所以直接这样配置IntelliJ IDEA 是可以找到的。但是假如你没有配置的话，这里可以选择你的Maven 安装目录。此外，这里不建议使用IDEA默认的。

Usersettings file / Local repository：我们还可以指定Maven 的settings.xml位置和本地仓库位置。

![1535476515067](C:\Users\ADMINI~1\AppData\Local\Temp\1535476515067.png)

Import Maven projects automatically：表示IntelliJ IDEA 会实时监控项目的pom.xml 文件，进行项目变动设置。

Automatically download：在Maven 导入依赖包的时候是否自动下载源码和文档。默认是没有勾选的，也不建议勾选，原因是这样可以加快项目从外网导入依赖包的速度，如果我们需要源码和文档的时候我们到时候再针对某个依赖包进行联网下载即可。IntelliJ IDEA 支持直接从公网下载源码和文档的。

VMoptions for importer：可以设置导入的VM 参数。一般这个都不需要主动改，除非项目真的导入太慢了我们再增大此参数。

# 十一、其他设置

### 1.**生成**javadoc

![1535476674565](C:\Users\ADMINI~1\AppData\Local\Temp\1535476674565.png)

![1535476682751](C:\Users\ADMINI~1\AppData\Local\Temp\1535476682751.png)

输入：

Locale：输入语言类型：zh_CN

Other command line arguments：-encoding UTF-8 -charset UTF-8

### 2. 缓存和索引的清理

IntelliJ IDEA 首次加载项目的时候，都会创建索引，而创建索引的时间跟项目的文件多少成正比。在IntelliJ IDEA 创建索引过程中即使你编辑了代码也是编译不了、运行不起来的，所以还是安安静静等IntelliJ IDEA 创建索引完成。

IntelliJ IDEA 的缓存和索引主要是用来加快文件查询，从而加快各种查找、代码提示等操作的速度，所以IntelliJ IDEA 的索引的重要性再强调一次也不为过。

但是，IntelliJ IDEA 的索引和缓存并不是一直会良好地支持IntelliJ IDEA 的，某些特殊条件下，IntelliJ IDEA 的缓存和索引文件也是会损坏的，比如：断电、蓝屏引起的强制关机，当你重新打开IntelliJ IDEA，很可能IntelliJ IDEA 会报各种莫名其妙错误，甚至项目打不开，IntelliJ IDEA 主题还原成默认状态。

即使没有断电、蓝屏，也会有莫名奇怪的问题的时候，也很有可能是IntelliJ IDEA 缓存和索引出现了问题，这种情况还不少。遇到此类问题也不用过多担心。我们可以清理缓存和索引。如下：

![1535476844097](C:\Users\ADMINI~1\AppData\Local\Temp\1535476844097.png)

![1535476851568](C:\Users\ADMINI~1\AppData\Local\Temp\1535476851568.png)

一般建议点击Invalidate and Restart，这样会比较干净。

上图警告：清除索引和缓存会使得IntelliJ IDEA 的Local History 丢失。所以如果你项目没有加入到版本控制，而你又需要你项目文件的历史更改记录，那你最好备份下你的LocalHistory 目录。目录地址在：C:\Users\当前登录的系统用户名\.IntelliJIdea14\system\LocalHistory 建议使用硬盘的全文搜索，这样效率更高。

通过上面方式清除缓存、索引本质也就是去删除C 盘下的system 目录下的对应的文件而已，所以如果你不用上述方法也可以删除整个system。当IntelliJ IDEA 再次启动项目的时候会重新创建新的system 目录以及对应项目缓存和索引。

### 4.插件的使用

在IntelliJ IDEA 的安装讲解中我们其实已经知道，IntelliJ IDEA 本身很多功能也都是通过插件的方式来实现的。

官网插件库：https://plugins.jetbrains.com/



![1535476949920](C:\Users\ADMINI~1\AppData\Local\Temp\1535476949920.png)

InstallJetBrains plugin：弹出IntelliJ IDEA 公司自行开发的插件仓库列表，供下载安装。

Browserepositories：弹出插件仓库中所有插件列表供下载安装。

Install plugin from disk：浏览本地的插件文件进行安装，而不是从服务器上下载并安装。

**需要特别注意的是：在国内的网络下，经常出现显示不了插件列表，或是显示了插件列表，无法下载完成安装。这时候请自行打开VPN，一般都可以得到解决。**

