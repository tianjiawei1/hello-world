## MybatisPlus

**简介**

[Mybatis-Plus](https://github.com/baomidou/mybatis-plus)（简称MP）是一个 [Mybatis](http://www.mybatis.org/mybatis-3/) 的增强工具，在 Mybatis 的基础上只做增强不做改变，为简化开发、提高效率而生。

**特性**

- 无侵入：Mybatis-Plus 在 Mybatis 的基础上进行扩展，只做增强不做改变，引入 Mybatis-Plus 不会对您现有的 Mybatis 构架产生任何影响，而且 MP 支持所有 Mybatis 原生的特性
- 依赖少：仅仅依赖 Mybatis 以及 Mybatis-Spring
- 损耗小：启动即会自动注入基本CURD，性能基本无损耗，直接面向对象操作
- 预防Sql注入：内置Sql注入剥离器，有效预防Sql注入攻击
- 通用CRUD操作：内置通用 Mapper、通用 Service，仅仅通过少量配置即可实现单表大部分 CRUD 操作，更有强大的条件构造器，满足各类使用需求
- 多种主键策略：支持多达4种主键策略（内含分布式唯一ID生成器），可自由配置，完美解决主键问题
- 支持ActiveRecord：支持 ActiveRecord 形式调用，实体类只需继承 Model 类即可实现基本 CRUD 操作
- 支持代码生成：采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、 Controller 层代码，支持模板引擎，更有超多自定义配置等您来使用（P.S. 比 Mybatis 官方的 Generator 更加强大！）
- 支持自定义全局通用操作：支持全局通用方法注入（ Write once, use anywhere ）
- 支持关键词自动转义：支持数据库关键词（order、key……）自动转义，还可自定义关键词
- 内置分页插件：基于Mybatis物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通List查询
- 内置性能分析插件：可输出Sql语句以及其执行时间，建议开发测试时启用该功能，能有效解决慢查询
- 内置全局拦截插件：提供全表 delete 、 update 操作智能分析阻断，预防误操作

### 一、入门

#### 1.简单示例（传统）

> 假设我们已存在一张 User 表，且已有对应的实体类 User，实现 User 表的 CURD 操作我们需要做什么呢？

```java
/** User 对应的 Mapper 接口 */
public interface UserMapper extends BaseMapper<User> { }
```

以上就是所需的所有操作，甚至不需要创建XML文件，我们如何使用它呢？

**基本CURD**

```java
// 初始化 影响行数
int result = 0;
// 初始化 User 对象
User user = new User();

// 插入 User (插入成功会自动回写主键到实体类)
user.setName("Tom");
result = userMapper.insert(user);

// 更新 User
user.setAge(18);
result = userMapper.updateById(user);

// 查询 User
User exampleUser = userMapper.selectById(user.getId());

// 查询姓名为‘张三’的所有用户记录
List<User> userList = userMapper.selectList(
        new EntityWrapper<User>().eq("name", "张三")
);

// 删除 User
result = userMapper.deleteById(user.getId());
```

以上是基本的 CRUD 操作，当然我们可用的 API 远不止这几个

**分页操作**

```java
// 分页查询 10 条姓名为‘张三’的用户记录
List<User> userList = userMapper.selectPage(
        new Page<User>(1, 10),
        new EntityWrapper<User>().eq("name", "张三")
);
```

> 现有一个需求，我们需要分页查询 User 表中，年龄在18~50之间性别为男且姓名为张三的所有用户，这时候我们该如何实现上述需求呢？

传统做法是 Mapper 中定义一个方法，然后在 Mapper 对应的 XML 中填写对应的 SELECT 语句，且我们还要集成分页，实现以上一个简单的需求，往往需要我们做很多重复单调的工作，普通的通用 Mapper 能够解决这类痛点么？

> 用 MP 的方式打开以上需求

```java
// 分页查询 10 条姓名为‘张三’、性别为男，且年龄在18至50之间的用户记录
List<User> userList = userMapper.selectPage(
        new Page<User>(1, 10),
        new EntityWrapper<User>().eq("name", "张三")
                .eq("sex", 0)
                .between("age", "18", "50")
);
```

以上操作，等价于

```sql
SELECT *
FROM sys_user
WHERE (name='张三' AND sex=0 AND age BETWEEN '18' AND '50')
LIMIT 0,10
```

Mybatis-Plus 通过 EntityWrapper（简称 EW，MP 封装的一个查询条件构造器）或者 Condition（与EW类似） 来让用户自由的构建查询条件，简单便捷，没有额外的负担，能够有效提高开发效率。

#### 2.简单示例（ActiveRecord）

ActiveRecord 一直广受动态语言（ PHP 、 Ruby 等）的喜爱，而 Java 作为准静态语言，对于 ActiveRecord 往往只能感叹其优雅，所以我们也在 AR 道路上进行了一定的探索，喜欢大家能够喜欢，也同时欢迎大家反馈意见与建议。

我们如何使用 AR 模式？

```java
@TableName("sys_user") // 注解指定表名
public class User extends Model<User> {

  ... // fields

  ... // getter and setter

  /** 指定主键 */
  @Override
  protected Serializable pkVal() {
      return this.id;
  }
}
```

我们仅仅需要继承 Model 类且实现主键指定方法 即可让实体开启 AR 之旅，开启 AR 之路后，我们如何使用它呢？

> 基本CRUD

```java
// 初始化 成功标识
boolean result = false;
// 初始化 User
User user = new User();

// 保存 User
user.setName("Tom");
result = user.insert();

// 更新 User
user.setAge(18);
result = user.updateById();

// 查询 User
User exampleUser = user.selectById();

// 查询姓名为‘张三’的所有用户记录
List<User> userList = user.selectList(
        new EntityWrapper<User>().eq("name", "张三")
);

// 删除 User
result = user.deleteById();
```

> 分页操作

```java
// 分页查询 10 条姓名为‘张三’的用户记录
List<User> userList = user.selectPage(
        new Page<User>(1, 10),
        new EntityWrapper<User>().eq("name", "张三")
).getRecords();
```

> 复杂操作

```java
// 分页查询 10 条姓名为‘张三’、性别为男，且年龄在18至50之间的用户记录
List<User> userList = user.selectPage(
        new Page<User>(1, 10),
        new EntityWrapper<User>().eq("name", "张三")
                .eq("sex", 0)
                .between("age", "18", "50")
).getRecords();
```

AR 模式提供了一种更加便捷的方式实现 CRUD 操作，其本质还是调用的 Mybatis 对应的方法，类似于语法糖。

通过以上两个简单示例，我们简单领略了 Mybatis-Plus 的魅力与高效率，值得注意的一点是：我们提供了强大的代码生成器，可以快速生成各类代码，真正的做到了即开即用。

#### 3.集成依赖

##### 3.1依赖配置

查询最高版本或历史版本方式：[Maven中央库](http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22com.baomidou%22%20AND%20a%3A%22mybatis-plus%22) | [Maven阿里库](http://maven.aliyun.com/nexus/#nexus-search;quick~mybatis-plus)

- pom中加入依赖

```java
<!-- mybatis mybatis-plus mybatis-spring mvc -->  
<dependency>  
    <groupId>com.baomidou</groupId>  
    <artifactId>mybatis-plus</artifactId>  
    <version>${mybatis-plus.version}</version>  
</dependency>  
<dependency>  
    <groupId>org.apache.velocity</groupId>  
    <artifactId>velocity-engine-core</artifactId>  
    <version>2.0</version>  
</dependency>  
```

注意：mybatis-plus的核心jar包中已集成了mybatis和mybatis-spring，所以为避免冲突，请勿再次引用这两个jar包。

PostgreSql 自定义 SQL 注入器 
sql-injector: com.baomidou.mybatisplus.mapper.LogicSqlInjector

示例代码：

##### 3.2 配置

- 在spring中配置MP

```xml
<bean id="sqlSessionFactory" class="com.baomidou.mybatisplus.spring.MybatisSqlSessionFactoryBean">
    <!-- 配置数据源 -->
    <property name="dataSource" ref="dataSource"/>
    <!-- 自动扫描 Xml 文件位置 -->
    <property name="mapperLocations" value="classpath:mybatis/*/*.xml"/>
    <!-- 配置 Mybatis 配置文件（可无） -->
    <property name="configLocation" value="classpath:mybatis/mybatis-config.xml"/>
    <!-- 配置包别名，支持通配符 * 或者 ; 分割 -->
    <property name="typeAliasesPackage" value="com.baomidou.springmvc.model"/>
    <!-- 枚举属性配置扫描，支持通配符 * 或者 ; 分割 -->
    <property name="typeEnumsPackage" value="com.baomidou.springmvc.entity.*.enums"/>

    <!-- 以上配置和传统 Mybatis 一致 -->
----------------------------------------------------------------------------------
    <!-- 插件配置 -->
    <property name="plugins">
        <array>
            <!-- 分页插件配置, 参考文档分页插件部分！！ -->
            <!-- 如需要开启其他插件，可配置于此 -->
        </array>
    </property>
------------------------------------------------------------------------------------
    <!-- MP 全局配置注入 -->
    <property name="globalConfig" ref="globalConfig"/>
</bean>

<!-- 定义 MP 全局策略 -->
<bean id="globalConfig" class="com.baomidou.mybatisplus.entity.GlobalConfiguration">
    <!-- 主键策略配置 -->
    <!-- 可选参数
        AUTO->`0`("数据库ID自增")
        INPUT->`1`(用户输入ID")
        ID_WORKER->`2`("全局唯一ID")
        UUID->`3`("全局唯一ID")
    -->
    <property name="idType" value="2"/>
------------------------------------------------------------------------------
    <!-- 数据库类型配置 -->
    <!-- 可选参数（默认mysql）
        MYSQL->`mysql`
        ORACLE->`oracle`
        DB2->`db2`
        H2->`h2`
        HSQL->`hsql`
        SQLITE->`sqlite`
        POSTGRE->`postgresql`
        SQLSERVER2005->`sqlserver2005`
        SQLSERVER->`sqlserver`
    -->
    <property name="dbType" value="oracle"/>

    <!-- 全局表为下划线命名设置 true -->
    <property name="dbColumnUnderline" value="true"/>
      <property name="sqlInjector">  
        <bean class="com.baomidou.mybatisplus.mapper.AutoSqlInjector" />  
    </property>  
</bean>
```

**注意**：只要做如上配置就可以正常使用mybatis了，不要重复配置。MP的配置和mybatis一样，都是配置一个sqlSessionFactory，只是现在所配置的类在原本的SqlSessionFactoryBean基础上做了增强。插件等配置请按需取舍。

### 二、核心功能

#### 1.代码生成器

在代码生成之前，首先进行配置，MP提供了大量的自定义设置，生成的代码完全能够满足各类型的需求，如果你发现配置不能满足你的需求，可以查看源码进行了解。

参数相关的配置，详见源码

> 主键策略选择

MP支持以下4中主键策略，可根据需求自行选用：

| 值               | 描述                                     |
| ---------------- | ---------------------------------------- |
| IdType.AUTO      | 数据库ID自增                             |
| IdType.INPUT     | 用户输入ID                               |
| IdType.ID_WORKER | 全局唯一ID，内容为空自动填充（默认配置） |
| IdType.UUID      | 全局唯一ID，内容为空自动填充             |

什么是**Sequence**？简单来说就是一个分布式高效有序ID生产黑科技工具，思路主要是来源于Twitter-Snowflake算法。

MP在Sequence的基础上进行部分优化，用于产生全局唯一ID，将ID_WORDER设置为默认配置。

> 表及字段命名策略选择

在MP中，建议数据库表名采用下划线命名方式，而表字段名采用驼峰命名方式。

这么做的原因是为了避免在对应实体类时产生的性能损耗，这样字段不用做映射就能直接和实体类对应。当然如果项目里不用考虑这点性能损耗，那么你采用下滑线也是没问题的，只需要在生成代码时配置dbColumnUnderline属性就可以。

> 如何生成代码 
> 方式一、代码生成引入模板引擎

```xml
<!-- 模板引擎 -->
<dependency>
    <groupId>org.apache.velocity</groupId>
    <artifactId>velocity-engine-core</artifactId>
    <version>2.0</version>
</dependency>

<!-- MP 核心库 -->
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus</artifactId>
    <version>最新版本</version>
</dependency>
```

> 代码生成、示例一

```java
import java.util.HashMap;
import java.util.Map;

import com.baomidou.mybatisplus.generator.AutoGenerator;
import com.baomidou.mybatisplus.generator.InjectionConfig;
import com.baomidou.mybatisplus.generator.config.DataSourceConfig;
import com.baomidou.mybatisplus.generator.config.GlobalConfig;
import com.baomidou.mybatisplus.generator.config.PackageConfig;
import com.baomidou.mybatisplus.generator.config.StrategyConfig;
import com.baomidou.mybatisplus.generator.config.rules.DbType;
import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;

/**
 * <p>
 * 代码生成器演示
 * </p>
 */
public class MpGenerator {

    /**
     * <p>
     * MySQL 生成演示
     * </p>
     */
    public static void main(String[] args) {
        AutoGenerator mpg = new AutoGenerator();

        // 全局配置
        GlobalConfig gc = new GlobalConfig();
        gc.setOutputDir("D://");
        gc.setFileOverride(true);
        gc.setActiveRecord(true);// 不需要ActiveRecord特性的请改为false
        gc.setEnableCache(false);// XML 二级缓存
        gc.setBaseResultMap(true);// XML ResultMap
        gc.setBaseColumnList(false);// XML columList
    	// .setKotlin(true) 是否生成 kotlin 代码
        gc.setAuthor("Yanghu");

        // 自定义文件命名，注意 %s 会自动填充表实体属性！
        // gc.setMapperName("%sDao");
        // gc.setXmlName("%sDao");
        // gc.setServiceName("MP%sService");
        // gc.setServiceImplName("%sServiceDiy");
        // gc.setControllerName("%sAction");
        mpg.setGlobalConfig(gc);

        // 数据源配置
        DataSourceConfig dsc = new DataSourceConfig();
        dsc.setDbType(DbType.MYSQL);
        dsc.setTypeConvert(new MySqlTypeConvert(){
            // 自定义数据库表字段类型转换【可选】
            @Override
            public DbColumnType processTypeConvert(String fieldType) {
                System.out.println("转换类型：" + fieldType);
        // 注意！！processTypeConvert 存在默认类型转换，如果不是你要的效果请自定义返回、非如下直接返回。
                return super.processTypeConvert(fieldType);
            }
        });
        dsc.setDriverName("com.mysql.jdbc.Driver");
        dsc.setUsername("root");
        dsc.setPassword("521");
        dsc.setUrl("jdbc:mysql://127.0.0.1:3306/mybatis-plus?characterEncoding=utf8");
        mpg.setDataSource(dsc);

        // 策略配置
        StrategyConfig strategy = new StrategyConfig();
    // strategy.setCapitalMode(true);// 全局大写命名 ORACLE 注意
        strategy.setTablePrefix(new String[] { "tlog_", "tsys_" });// 此处可以修改为您的表前缀
        strategy.setNaming(NamingStrategy.underline_to_camel);// 表名生成策略
        // strategy.setInclude(new String[] { "user" }); // 需要生成的表
        // strategy.setExclude(new String[]{"test"}); // 排除生成的表
        // 自定义实体父类
        // strategy.setSuperEntityClass("com.baomidou.demo.TestEntity");
        // 自定义实体，公共字段
        // strategy.setSuperEntityColumns(new String[] { "test_id", "age" });
        // 自定义 mapper 父类
        // strategy.setSuperMapperClass("com.baomidou.demo.TestMapper");
        // 自定义 service 父类
        // strategy.setSuperServiceClass("com.baomidou.demo.TestService");
        // 自定义 service 实现类父类
        // strategy.setSuperServiceImplClass("com.baomidou.demo.TestServiceImpl");
        // 自定义 controller 父类
        // strategy.setSuperControllerClass("com.baomidou.demo.TestController");
        // 【实体】是否生成字段常量（默认 false）
        // public static final String ID = "test_id";
        // strategy.setEntityColumnConstant(true);
        // 【实体】是否为构建者模型（默认 false）
        // public User setName(String name) {this.name = name; return this;}
        // strategy.setEntityBuilderModel(true);
        mpg.setStrategy(strategy);

        // 包配置
        PackageConfig pc = new PackageConfig();
        pc.setParent("com.baomidou");
        pc.setModuleName("test");
        mpg.setPackageInfo(pc);

        // 注入自定义配置，可以在 VM 中使用 cfg.abc 【可无】
        InjectionConfig cfg = new InjectionConfig() {
            @Override
            public void initMap() {
                Map<String, Object> map = new HashMap<String, Object>();
                map.put("abc", this.getConfig().getGlobalConfig().getAuthor() + "-mp");
                this.setMap(map);
            }
        };

        // 自定义 xxList.jsp 生成
        List<FileOutConfig> focList = new ArrayList<FileOutConfig>();
        focList.add(new FileOutConfig("/template/list.jsp.vm") {
            @Override
            public String outputFile(TableInfo tableInfo) {
                // 自定义输入文件名称
                return "D://my_" + tableInfo.getEntityName() + ".jsp";
            }
        });
        cfg.setFileOutConfigList(focList);
        mpg.setCfg(cfg);

    // 调整 xml 生成目录演示
         focList.add(new FileOutConfig("/templates/mapper.xml.vm") {
            @Override
            public String outputFile(TableInfo tableInfo) {
                return "/develop/code/xml/" + tableInfo.getEntityName() + ".xml";
            }
        });
        cfg.setFileOutConfigList(focList);
        mpg.setCfg(cfg);

        // 关闭默认 xml 生成，调整生成 至 根目录
        TemplateConfig tc = new TemplateConfig();
        tc.setXml(null);
        mpg.setTemplate(tc);

        // 自定义模板配置，可以 copy 源码 mybatis-plus/src/main/resources/templates 下面内容修改，
        // 放置自己项目的 src/main/resources/templates 目录下, 默认名称一下可以不配置，也可以自定义模板名称
        // TemplateConfig tc = new TemplateConfig();
        // tc.setController("...");
        // tc.setEntity("...");
        // tc.setMapper("...");
        // tc.setXml("...");
        // tc.setService("...");
        // tc.setServiceImpl("...");
    // 如上任何一个模块如果设置 空 OR Null 将不生成该模块。
        // mpg.setTemplate(tc);

        // 执行生成
        mpg.execute();

        // 打印注入设置【可无】
        System.err.println(mpg.getCfg().getMap().get("abc"));
    }

}
```

> 代码生成、示例二

```java
new AutoGenerator().setGlobalConfig(
    ...
).setDataSource(
    ...
).setStrategy(
    ...
).setPackageInfo(
    ...
).setCfg(
    ...
).setTemplate(
    ...
).execute();
```

> 其他方式、 Maven插件生成

待补充（Maven代码生成插件 待完善）

#### 2.通用 CRUD

##### 简单介绍

> 实体无注解化设置，表字段如下规则，主键叫 id 可无注解大写小如下规则。

1、驼峰命名 【 无需处理 】

2、全局配置： 下划线命名 dbColumnUnderline 设置 true , 大写 isCapitalMode 设置 true

##### 注解说明

> 表名注解 @TableName

- com.baomidou.mybatisplus.annotations.TableName

| 值        | 描述                      |
| --------- | ------------------------- |
| value     | 表名（ 默认空 ）          |
| resultMap | xml 字段映射 resultMap ID |

> 主键注解 @TableId

- com.baomidou.mybatisplus.annotations.TableId

| 值    | 描述                                                      |
| ----- | --------------------------------------------------------- |
| value | 字段值（驼峰命名方式，该值可无）                          |
| type  | 主键 ID 策略类型（ 默认 INPUT ，全局开启的是 ID_WORKER ） |

> 字段注解 @TableField

- com.baomidou.mybatisplus.annotations.TableField

| 值       | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| value    | 字段值（驼峰命名方式，该值可无）                             |
| el       | 是否为数据库表字段（ 默认 true 存在，false 不存在 ）         |
| exist    | 是否为数据库表字段（ 默认 true 存在，false 不存在 ）         |
| strategy | 字段验证 （ 默认 非 null 判断，查看 com.baomidou.mybatisplus.enums.FieldStrategy ） |
| fill     | 字段填充标记 （ 配合自动填充使用 ）                          |

> 序列主键策略 注解 @KeySequence

- com.baomidou.mybatisplus.annotations.KeySequence

| 值    | 描述     |
| ----- | -------- |
| value | 序列名   |
| clazz | id的类型 |

> 乐观锁标记注解 @Version

- com.baomidou.mybatisplus.annotations.Version

#### 3.条件构造器

实体包装器，用于处理 sql 拼接，排序，实体参数查询等！

> 补充说明： 使用的是数据库字段，不是Java属性!

实体包装器 EntityWrapper 继承 Wrapper

- 翻页查询

```java
public Page<T> selectPage(Page<T> page, EntityWrapper<T> entityWrapper) {
  if (null != entityWrapper) {
      entityWrapper.orderBy(page.getOrderByField(), page.isAsc());
  }
  page.setRecords(baseMapper.selectPage(page, entityWrapper));
  return page;
}
```

- 拼接 sql 方式 一

```java
@Test
public void testTSQL11() {
    /*
     * 实体带查询使用方法  输出看结果
     */
    EntityWrapper<User> ew = new EntityWrapper<User>();
    ew.setEntity(new User(1));
    ew.where("user_name={0}", "'zhangsan'").and("id=1")
            .orNew("user_status={0}", "0").or("status=1")
            .notLike("user_nickname", "notvalue")
            .andNew("new=xx").like("hhh", "ddd")
            .andNew("pwd=11").isNotNull("n1,n2").isNull("n3")
            .groupBy("x1").groupBy("x2,x3")
            .having("x1=11").having("x3=433")
            .orderBy("dd").orderBy("d1,d2");
    System.out.println(ew.getSqlSegment());
}
```

- 拼接 sql 方式 二

```java
int buyCount = selectCount(Condition.create()
                .setSqlSelect("sum(quantity)")
                .isNull("order_id")
                .eq("user_id", 1)
                .eq("type", 1)
                .in("status", new Integer[]{0, 1})
                .eq("product_id", 1)
                .between("created_time", startDate, currentDate)
                .eq("weal", 1));
```

- 自定义 SQL 方法如何使用 Wrapper

mapper java 接口方法

List selectMyPage(RowBounds rowBounds, @Param(“ew”) Wrapper wrapper);

mapper xml 定义

```xml
<select id="selectMyPage" resultType="User">
  SELECT * FROM user 
  <where>
  ${ew.sqlSegment}
  </where>
</select>
```

### 三、插件扩展

#### 1.分页插件

- 自定义查询语句分页（自己写sql/mapper）
- spring 注入 mybatis 配置分页插件

```xml
<plugins>
    <!--
     | 分页插件配置
     | 插件提供二种方言选择：1、默认方言 2、自定义方言实现类，两者均未配置则抛出异常！
     | overflowCurrent 溢出总页数，设置第一页 默认false
     | optimizeType Count优化方式 （ 版本 2.0.9 改为使用 jsqlparser 不需要配置 ）
     | -->
    <!-- 注意!! 如果要支持二级缓存分页使用类 CachePaginationInterceptor 默认、建议如下！！ -->
    <plugin interceptor="com.baomidou.mybatisplus.plugins.PaginationInterceptor">
        <property name="sqlParser" ref="自定义解析类、可以没有" />
        <property name="localPage" value="默认 false 改为 true 开启了 pageHeper 支持、可以没有" />
        <property name="dialectClazz" value="自定义方言类、可以没有" />
    </plugin>
</plugins>


```

```java 
//Spring boot方式
@EnableTransactionManagement
@Configuration
@MapperScan("com.baomidou.cloud.service.*.mapper*")
public class MybatisPlusConfig {
    /**
     * 分页插件
     */
    @Bean
    public PaginationInterceptor paginationInterceptor() {
        return new PaginationInterceptor();
    }
}
```

##### 1.1方式一 、传参区分模式【推荐】

- UserMapper.java 方法内容

```java
public interface UserMapper{//可以继承或者不继承BaseMapper
    /**
     * <p>
     * 查询 : 根据state状态查询用户列表，分页显示
     * </p>
     *
     * @param page
     * 翻页对象，可以作为 xml 参数直接使用，传递参数 Page 即自动分页
     * @param state
     * 状态
     * @return
     */
    List<User> selectUserList(Pagination page, Integer state);
}
```

- UserServiceImpl.java 调用翻页方法，需要 page.setRecords 回传给页面

```java
public Page<User> selectUserPage(Page<User> page, Integer state) {
    page.setRecords(userMapper.selectUserList(page, state));
    return page;
}
```

- UserMapper.xml 等同于编写一个普通 list 查询，mybatis-plus 自动替你分页

```xml
<select id="selectUserList" resultType="User">
    SELECT * FROM user WHERE state=#{state}
</select>
```

##### 1.2方式二、ThreadLocal 模式【容易出错，不推荐】

- PageHelper 使用方式如下：

```java
// 开启分页
PageHelper.startPage(1, 2);
List<User> data = userService.findAll(params);
// 获取总条数
int total = PageHelper.getTotal();
// 获取总条数，并释放资源
int total = PageHelper.freeTotal();
```

#### 2.执行分析插件

> SQL 执行分析拦截器【 目前只支持 MYSQL-5.6.3 以上版本 】，作用是分析 处理 DELETE UPDATE 语句， 防止小白或者恶意 delete update 全表操作！

```xml
<plugins>
    <!-- SQL 执行分析拦截器 stopProceed 发现全表执行 delete update 是否停止运行 -->
    <plugin interceptor="com.baomidou.mybatisplus.plugins.SqlExplainInterceptor">
        <property name="stopProceed" value="false" />
    </plugin>
</plugins>
```

> 注意！参数说明

- 参数：stopProceed 发现执行全表 delete update 语句是否停止执行
- 注意！该插件只用于开发环境，不建议生产环境使用。。。

#### 3.性能分析插件

> 性能分析拦截器，用于输出每条 SQL 语句及其执行时间

- 使用如下：

```xml
<plugins>
    ....

    <!-- SQL 执行性能分析，开发环境使用，线上不推荐。 maxTime 指的是 sql 最大执行时长 -->
    <plugin interceptor="com.baomidou.mybatisplus.plugins.PerformanceInterceptor">
        <property name="maxTime" value="100" />
        <!--SQL是否格式化 默认false-->
        <property name="format" value="true" />
    </plugin>
</plugins>

```

```java 
//Spring boot方式
@EnableTransactionManagement
@Configuration
@MapperScan("com.baomidou.cloud.service.*.mapper*")
public class MybatisPlusConfig {
    /**
     * SQL执行效率插件
     */
    @Bean
    @Profile({"dev","test"})// 设置 dev test 环境开启
    public PerformanceInterceptor performanceInterceptor() {
        return new PerformanceInterceptor();
    }
}
```

> 注意！参数说明

- 参数：maxTime SQL 执行最大时长，超过自动停止运行，有助于发现问题。
- 参数：format SQL SQL是否格式化，默认false。

#### 3.乐观锁插件

意图：当要更新一条记录的时候，希望这条记录没有被别人更新

乐观锁实现方式：

- 取出记录时，获取当前version
- 更新时，带上这个version
- 执行更新时， set version = yourVersion+1 where version = yourVersion
- 如果version不对，就更新失败

```xml
<bean class="com.baomidou.mybatisplus.plugins.OptimisticLockerInterceptor"/>
```

**注解实体字段 @Version 必须要！**

```java
public class User {
    @Version
    private Integer version;

    ...
}
```

> 特别说明： **仅支持int,Integer,long,Long,Date,Timestamp

示例Java代码

```java
int id = 100;
int version = 2;

User u = new User();
u.setId(id);
u.setVersion(version);
u.setXXX(xxx);

if(userService.updateById(u)){
    System.out.println("Update successfully");
}else{
    System.out.println("Update failed due to modified by others");
}
```

示例SQL原理

```sql
update tbl_user set name='update',version=3 where id=100 and version=2;
```

#### 4.XML文件热加载

> 开启动态加载 mapper.xml

- 多数据源配置多个 MybatisMapperRefresh 启动 bean

```
参数说明：
      sqlSessionFactory:session工厂
      mapperLocations:mapper匹配路径
      enabled:是否开启动态加载  默认:false
      delaySeconds:项目启动延迟加载时间  单位：秒  默认:10s
      sleepSeconds:刷新时间间隔  单位：秒 默认:20s
  提供了两个构造,挑选一个配置进入spring配置文件即可：

构造1:
    <bean class="com.baomidou.mybatisplus.spring.MybatisMapperRefresh">
        <constructor-arg name="sqlSessionFactory" ref="sqlSessionFactory"/>
        <constructor-arg name="mapperLocations" value="classpath*:mybatis/mappers/*/*.xml"/>
        <constructor-arg name="enabled" value="true"/>
    </bean>

构造2:
    <bean class="com.baomidou.mybatisplus.spring.MybatisMapperRefresh">
        <constructor-arg name="sqlSessionFactory" ref="sqlSessionFactory"/>
        <constructor-arg name="mapperLocations" value="classpath*:mybatis/mappers/*/*.xml"/>
        <constructor-arg name="delaySeconds" value="10"/>
        <constructor-arg name="sleepSeconds" value="20"/>
        <constructor-arg name="enabled" value="true"/>
    </bean>
```

#### 5.自定义全局操作

> 自定义注入全表删除方法 deteleAll 
> 自定义 MySqlInjector 注入类 java 代码如下：

```java
public class MySqlInjector extends AutoSqlInjector {

    @Override
    public void inject(Configuration configuration, MapperBuilderAssistant builderAssistant, Class<?> mapperClass,
            Class<?> modelClass, TableInfo table) {
        /* 添加一个自定义方法 */
        deleteAllUser(mapperClass, modelClass, table);
    }

    public void deleteAllUser(Class<?> mapperClass, Class<?> modelClass, TableInfo table) {

        /* 执行 SQL ，动态 SQL 参考类 SqlMethod */
        String sql = "delete from " + table.getTableName();

        /* mapper 接口方法名一致 */
        String method = "deleteAll";
        SqlSource sqlSource = languageDriver.createSqlSource(configuration, sql, modelClass);
        this.addMappedStatement(mapperClass, method, sqlSource, SqlCommandType.DELETE, Integer.class);
    }

}
```

> 当然你的 mapper.java 接口类需要申明使用方法 deleteAll 如下

```java
public interface UserMapper extends BaseMapper<User> {

    /**
     * 自定义注入方法
     */
    int deleteAll();

}
```

> 最后一步注入启动

```xml
<!-- 定义 MP 全局策略，安装集成文档部分结合 -->
<bean id="globalConfig" class="com.baomidou.mybatisplus.entity.GlobalConfiguration">
    .....

  <!-- 自定义注入 deleteAll 方法  -->
  <property name="sqlInjector" ref="mySqlInjector" />
</bean>

<!-- 自定义注入器 -->
<bean id="mySqlInjector" class="com.baomidou.test.MySqlInjector" />
```

- 完成如上几步共享，注入完成！可以开始使用了。。。

#### 6.公共字段自动填充

- 实现元对象处理器接口： com.baomidou.mybatisplus.mapper.IMetaObjectHandler
- 注解填充字段 @TableField(.. fill = FieldFill.INSERT) 生成器策略部分也可以配置！

```java
public class User {

    // 注意！这里需要标记为填充字段
    @TableField(.. fill = FieldFill.INSERT)
    private String fillField;
    ....
}
```

- 自定义实现类 MyMetaObjectHandler

```java
/**  自定义填充公共 name 字段  */
public class MyMetaObjectHandler extends MetaObjectHandler {

    /**
     * 测试 user 表 name 字段为空自动填充
     */
    public void insertFill(MetaObject metaObject) {
        // 更多查看源码测试用例
        System.out.println("*************************");
        System.out.println("insert fill");
        System.out.println("*************************");

        // 测试下划线
        Object testType = getFieldValByName("testType", metaObject);//mybatis-plus版本2.0.9+
        System.out.println("testType=" + testType);
        if (testType == null) {
            setFieldValByName("testType", 3, metaObject);//mybatis-plus版本2.0.9+
        }
    }

    @Override
    public void updateFill(MetaObject metaObject) {
        //更新填充
        System.out.println("*************************");
        System.out.println("update fill");
        System.out.println("*************************");
        //mybatis-plus版本2.0.9+
        setFieldValByName("lastUpdatedDt", new Timestamp(System.currentTimeMillis()), metaObject);
    }
}
```

> spring 启动注入 MyMetaObjectHandler 配置

```xml
<!-- MyBatis SqlSessionFactoryBean 配置 -->
<bean id="sqlSessionFactory" class="com.baomidou.mybatisplus.spring.MybatisSqlSessionFactoryBean">
    <property name="globalConfig" ref="globalConfig"></property>
</bean>
<bean id="globalConfig" class="com.baomidou.mybatisplus.entity.GlobalConfiguration">
    <!-- 公共字段填充处理器 -->
    <property name="metaObjectHandler" ref="myMetaObjectHandler" />
</bean>
<!-- 自定义处理器 -->
<bean id="myMetaObjectHandler" class="com.baomidou.test.MyMetaObjectHandler" />
```

#### 7.逻辑删除

##### 7.1开启配置

1、修改 集成 全局注入器为 LogicSqlInjector 
2、全局注入值： 
logicDeleteValue // 逻辑删除全局值 
logicNotDeleteValue // 逻辑未删除全局值

3、逻辑删除的字段需要注解 @TableLogic

[Mybatis-Plus逻辑删除视频教程](http://v.youku.com/v_show/id_XMjc4ODY0MDI5Ng==.html?spm=a2hzp.8244740.userfeed.5!2~5~5~5!3~5~A)

#### 8.全局配置注入LogicSqlInjector Java Config方式：

```java
@Bean
public GlobalConfiguration globalConfiguration() {
    GlobalConfiguration conf = new GlobalConfiguration(new LogicSqlInjector());
    conf.setLogicDeleteValue("-1");
    conf.setLogicNotDeleteValue("1");
    conf.setIdType(2);
    return conf;
}
```

XML配置方式：

```xml
<bean id="globalConfig" class="com.baomidou.mybatisplus.entity.GlobalConfiguration">
    <property name="sqlInjector" ref="logicSqlInjector" />
    <property name="logicDeleteValue" value="-1" />
    <property name="logicNotDeleteValue" value="1" />
    <property name="idType" value="2" />
</bean>

<bean id="logicSqlInjector" class="com.baomidou.mybatisplus.mapper.LogicSqlInjector" />
```

#### 9.逻辑删除实体

```java
@TableName("tbl_user")
public class UserLogicDelete {

    private Long id;
    ...

    @TableField(value = "delete_flag")
    @TableLogic
    private Integer deleteFlag;
}
```

**逻辑删除效果**

> 会在mp自带查询和更新方法的sql后面，追加『逻辑删除字段』=『LogicNotDeleteValue默认值』 删除方法: deleteById()和其他delete方法, 底层SQL调用的是update tbl_xxx set 『逻辑删除字段』=『logicDeleteValue默认值』

##### 9.1.多数据源

> 第一步：扩展Spring的AbstractRoutingDataSource抽象类，实现动态数据源。 AbstractRoutingDataSource中的抽象方法determineCurrentLookupKey是实现数据源的route的核心，这里对该方法进行Override。 【上下文DbContextHolder为一线程安全的ThreadLocal】具体代码如下：

```java
public class DynamicDataSource extends AbstractRoutingDataSource {

/**
 * 取得当前使用哪个数据源
 * @return
 */
@Override
protected Object determineCurrentLookupKey() {
    return DbContextHolder.getDbType();
}
}

public class DbContextHolder {
private static final ThreadLocal contextHolder = new ThreadLocal<>();

/**
 * 设置数据源
 * @param dbTypeEnum
 */
public static void setDbType(DBTypeEnum dbTypeEnum) {
    contextHolder.set(dbTypeEnum.getValue());
}

/**
 * 取得当前数据源
 * @return
 */
public static String getDbType() {
    return contextHolder.get();
}

/**
 * 清除上下文数据
 */
public static void clearDbType() {
    contextHolder.remove();
}
}

public enum DBTypeEnum {
one("dataSource_one"), two("dataSource_two");
private String value;

DBTypeEnum(String value) {
    this.value = value;
}

public String getValue() {
    return value;
}
}
```

> 第二步：配置动态数据源将DynamicDataSource Bean加入到Spring的上下文xml配置文件中去，同时配置DynamicDataSource的targetDataSources(多数据源目标)属性的Map映射。 代码如下【省略了dataSource_one和dataSource_two的配置】：

```xml
<bean id="dataSource" class="com.miyzh.dataclone.db.DynamicDataSource">
    <property name="targetDataSources">
        <map key-type="java.lang.String">
            <entry key="dataSource_one" value-ref="dataSource_one" />
            <entry key="dataSource_two" value-ref="dataSource_two" />
        </map>
    </property>
    <property name="defaultTargetDataSource" ref="dataSource_two" />
</bean>
```

> 第三步：使用动态数据源：DynamicDataSource是继承与AbstractRoutingDataSource，而AbstractRoutingDataSource又是继承于org.springframework.jdbc.datasource.AbstractDataSource，AbstractDataSource实现了统一的DataSource接口，所以DynamicDataSource同样可以当一个DataSource使用。

```java
@Test 
public void test() {
DbContextHolder.setDbType(DBTypeEnum.one);
List userList = iUserService.selectList(new EntityWrapper());
for (User user : userList) {
log.debug(user.getId() + "#" + user.getName() + "#" + user.getAge());
}

    DbContextHolder.setDbType(DBTypeEnum.two);
    List<IdsUser> idsUserList = iIdsUserService.selectList(new EntityWrapper<IdsUser>());
    for (IdsUser idsUser : idsUserList) {
        log.debug(idsUser.getMobile() + "#" + idsUser.getUserName());
    }
}
```

> 说明：

- 1、事务管理：使用动态数据源的时候，可以看出和使用单数据源的时候相比，在使用配置上几乎没有差别，在进行性事务管理配置的时候也没有差别：
- 2、通过扩展Spring的AbstractRoutingDataSource可以很好的实现多数据源的rout效果，而且对扩展更多的数据源有良好的伸缩性，只要增加数据源和修改DynamicDataSource的targetDataSources属性配置就好。在数据源选择控制上，可以采用手动控制(业务逻辑并不多的时候)，也可以很好的用AOP的@Aspect在Service的入口加入一个切面@Pointcut，在@Before里判断JoinPoint的类容选定特定的数据源。

##### 9.2 Sequence主键

实体主键支持Sequence @since 2.0.8

- oracle主键策略配置Sequence
- GlobalConfiguration配置KeyGenerator

```java
GlobalConfiguration gc = new GlobalConfiguration();
  //gc.setDbType("oracle");//不需要这么配置了
  gc.setKeyGenerator(new OracleKeyGenerator());
```

- 实体类配置主键Sequence,指定主键@TableId(type=IdType.INPUT)//不能使用AUTO

```java
@TableName("TEST_SEQUSER")
@KeySequence("SEQ_TEST")//类注解
public class TestSequser{
  @TableId(value = "ID", type = IdType.INPUT)
  private Long id;

}
```

- 支持父类定义@KeySequence, 子类使用

```java
@KeySequence("SEQ_TEST")
public abstract class Parent{

}
public class Child extends Parent{

}
```

以上步骤就可以使用Sequence当主键了。

##### 9.3 多租户 SQL 解析器

- 这里配合 分页拦截器 使用， spring boot 例子配置如下：

```java
@Bean
public PaginationInterceptor paginationInterceptor() {
    PaginationInterceptor paginationInterceptor = new PaginationInterceptor();
    paginationInterceptor.setLocalPage(true);// 开启 PageHelper 的支持
    /*
     * 【测试多租户】 SQL 解析处理拦截器<br>
     * 这里固定写成住户 1 实际情况你可以从cookie读取，因此数据看不到 【 麻花藤 】 这条记录（ 注意观察 SQL ）<br>
     */
    List<ISqlParser> sqlParserList = new ArrayList<>();
    TenantSqlParser tenantSqlParser = new TenantSqlParser();
    tenantSqlParser.setTenantHandler(new TenantHandler() {
        @Override
        public Expression getTenantId() {
            return new LongValue(1L);
        }

        @Override
        public String getTenantIdColumn() {
            return "tenant_id";
        }

        @Override
        public boolean doTableFilter(String tableName) {
            // 这里可以判断是否过滤表
            /*
            if ("user".equals(tableName)) {
                return true;
            }*/
            return false;
        }
    });
    sqlParserList.add(tenantSqlParser);
    paginationInterceptor.setSqlParserList(sqlParserList);
    paginationInterceptor.setSqlParserFilter(new ISqlParserFilter() {
        @Override
        public boolean doFilter(MetaObject metaObject) {
            MappedStatement ms = PluginUtils.getMappedStatement(metaObject);
            // 过滤自定义查询此时无租户信息约束【 麻花藤 】出现
            if ("com.baomidou.springboot.mapper.UserMapper.selectListBySQL".equals(ms.getId())) {
                return true;
            }
            return false;
        }
    });
    return paginationInterceptor;
}
```

##### 9.4 通用枚举扫描并自动关联注入

###### 1、自定义枚举实现 IEnum 接口，申明自动注入为通用枚举转换处理器

> 枚举属性，必须实现 IEnum 接口如下：

```java
public enum AgeEnum implements IEnum {
    ONE(1, "一岁"),
    TWO(2, "二岁");

    private int value;
    private String desc;

    AgeEnum(final int value, final String desc) {
        this.value = value;
        this.desc = desc;
    }

    @Override
    public Serializable getValue() {
        return this.value;
    }

    @JsonValue // Jackson 注解，可序列化该属性为中文描述【可无】
    public String getDesc(){
        return this.desc;
    }
}
```

###### 2、配置扫描通用枚举

- 注意!! spring mvc 配置参考，安装集成 MybatisSqlSessionFactoryBean 枚举包扫描，spring boot 例子配置如下：

> 配置文件 resources/application.yml

```xml
mybatis-plus:
    # 支持统配符 * 或者 ; 分割
    typeEnumsPackage: com.baomidou.springboot.entity.enums
  ....
```

##### 9.5 MybatisX idea 快速开发插件

> MybatisX 辅助 idea 快速开发插件，为效率而生。

- 官方安装： File -> Settings -> Plugins -> Browse Repositories.. 输入 mybatisx 安装下载
- Jar 安装： File -> Settings -> Plugins -> Install plugin from disk.. 选中 mybatisx..jar 安装

###### 1功能

- java xml 调回跳转，mapper 方法自动生成 xml

###### 2计划支持

- 连接数据源之后xml里自动提示字段
- sql 增删改查
- 集成 MP 代码生成
- 其他