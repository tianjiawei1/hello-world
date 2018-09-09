# Springcloud

## 一、简介

    springCloud是基于SpringBoot的一整套实现微服务的框架。他提供了微服务开发所需的配置管理、服务发现、断路器、智能路由、微代理、控制总线、全局锁、决策竞选、分布式会话和集群状态管理等组件。最重要的是，跟spring boot框架一起使用的话，会让你开发微服务架构的云服务非常好的方便。

**Eureka:  云端负载均衡，一个基于 REST 的服务，用于定位服务，以实现云端的负载均衡和中间层服务器的故障转移。**

```tx
Eureka Server
Eureka Client
Eureka高可用
服务发现机制 	
```

**Config:配置管理开发工具包，可以让你把配置放到远程服务器，目前支持本地存储、Git以及Subversion。**

```tx
Config Server
Config Client
Spring Cloud Bus(结合RabbitMQ) 自动刷新 ：事件、消息总线，用于在集群（例如，配置变化事件）中传播状态变化，可与Spring Cloud Config联合实现热部署。
```

**Ribbon:**

```tx
RestTemplate
Feign
Ribbon(分析源码，了解底层）
```

**Zuul:边缘服务工具，是提供动态路由，监控，弹性，安全等的边缘服务。**

```tx
动态路由
校验
```

**Hystirx:容错管理工具，旨在通过控制服务和第三方库的节点,从而对延迟和故障提供更强大的容错能力。**

```tz
Hystirx Dashboar 熔断机制
```

**容器编排和服务**

```txt
docker+Rancher
Spring Cloud +Zipkin
```

环境参数：idea docker springboot rancher springcloud

 ### SpringCloud特点

1：约定优于配置

2：开箱即用、快速启动

3：适用于各种环境

4：轻量级的组件

5：组件支持丰富，功能齐全

**原生云是应用程序开发的一种风格，鼓励在持续交付和价值驱动领域的最佳实践。 Spring Cloud的很多特性是基于Spring Boot的。更多的是由两个库实现：Spring Cloud Context and Spring Cloud Commons。**

#### Spring Cloud Context: 应用上下文服务

Spring Boot关于使用Spring构建应用有硬性规定：通用的配置文件在固定的位置，通用管理终端，监控任务。建立在这个基础上，Spring Cloud增加了一些额外的特性。

##### 1 引导应用程序上下文

Spring Cloud会创建一个“bootstrap”的上下文，这是主应用程序的父上下文。对应的配置文件拥有最高优先级，并且，默认不能被本地配置文件覆盖。对应的文件名bootstrap.yml或bootstrap.properties。

可通过设置`spring.cloud.bootstrap.enabled=false`来禁止bootstrap进程。

##### 2 应用上下文层级结构

当用`SpringApplication`或`SpringApplicationBuilder`创建应用程序上下文时，bootstrap上下文将作为父上下文被添加进去，子上下文将继承父上下文的属性。

子上下文的配置信息可覆盖父上下文的配置信息。

##### 3 修改Bootstrap配置文件位置

`spring.cloud.bootstrap.name`（默认是bootstrap），或者`spring.cloud.bootstrap.location`（默认是空）

##### 4 覆盖远程配置文件的值

```
spring.cloud.config.allowOverride=true`
`spring.cloud.config.overrideNone=true`
`spring.cloud.config.overrideSystemProperties=false
```

##### 5 定制Bootstrap配置

在`/META-INF/spring.factories`的key为`org.springframework.cloud.bootstrap.BootstrapConfiguration`，定义了Bootstrap启动的组件。

在主应用程序启动之前，一开始Bootstrap上下文创建在spring.factories文件中的组件，然后是`@Beans`类型的bean。

##### 6 定制Bootstrap属性来源

关键点：spring.factories、PropertySourceLocator

##### 7 环境改变

应用程序可通过`EnvironmentChangedEvent`监听应用程序并做出响应。

##### 8 Refresh Scope

Spring的bean被@RefreshScope将做特殊处理，可用于刷新bean的配置信息。

注意

- 需要添加依赖“org.springframework.boot.spring-boot-starter-actuator”
- 目前我只在@Controller测试成功
- 需要自己发送POST请求`/refresh`
- 修改配置文件即可

##### 9 加密和解密

Spring Cloud可对配置文件的值进行加密。
如果有"Illegal key size"异常，那么需要安装JCE。

##### 10 服务点

除了Spring Boot提供的服务点，Spring Cloud也提供了一些服务点用于管理，注意都是POST请求

- `/env`：更新`Environment`、重新绑定`@ConfigurationProperties`跟日志级别
- `/refresh`重新加载配置文件，刷新标记`@RefreshScope`的bean
- `/restart`重启应用，默认不可用
- 生命周期方法：`/pause`、`/resume`

#### Spring Cloud Commons:通用抽象

服务发现、负载均衡、熔断机制这种模式为Spring Cloud客户端提供了一个通用的抽象层。

##### 1 RestTemplate作为负载均衡客户端

通过`@Bean`跟`@LoadBalanced`指定`RestTemplate`。注意URI需要使用虚拟域名（如服务名，不能用域名）。

如下：

```java
@Configuration
public class MyConfiguration {

    @LoadBalanced
    @Bean
    RestTemplate restTemplate() {
        return new RestTemplate();
    }
}

public class MyClass {
    @Autowired
    private RestTemplate restTemplate;

    public String doOtherStuff() {
        String results = restTemplate.getForObject("http://stores/stores", String.class);
        return results;
    }
}
```

##### 2 多个RestTemplate对象

注意`@Primary`注解的使用。

```java
@Configuration
public class MyConfiguration {

    @LoadBalanced
    @Bean
    RestTemplate loadBalanced() {
        return new RestTemplate();
    }

    @Primary
    @Bean
    RestTemplate restTemplate() {
        return new RestTemplate();
    }
}

public class MyClass {
    @Autowired
    private RestTemplate restTemplate;

    @Autowired
    @LoadBalanced
    private RestTemplate loadBalanced;

    public String doOtherStuff() {
        return loadBalanced.getForObject("http://stores/stores", String.class);
    }

    public String doStuff() {
        return restTemplate.getForObject("http://example.com", String.class);
    }
}
```

##### 3 忽略网络接口

忽略确定名字的服务发现注册，支持正则表达式配置。

## 二、Eyreka注册服务中心

**Eureka注册服务中心 三个重要的角色：服务注册中心、服务提供者、服务消费者。**

- 单点模式 注册服务中心只有一个。

  ```xml
  #指定端口号
  server.port=1111
  #是否优先使用IP地址作为主机名的标识  默认为false
  eureka.instance.preferIpAddress=true
  #是否注册到eureka
  eureka.client.register-with-eureka=false
  #是否从eureka获取注册信息
  eureka.client.fetch-registry=false
  #eureka服务器的地址（注意：地址最后面的 /eureka/ 这个是固定值）
  eureka.client.serviceUrl.defaultZone=http://localhost:${server.port}/eureka/
  ```

- 高可用模式 服务中心可用将自己作为服务提供者，注册到相关的服务中心去。服务中心可以有多个，集群的方式。

  ```xml
  #application-peer1.properties
  spring.application.name=eureka-server
  server.port=1111
  eureka.instance.hostname=peer1
  eureka.client.serviceUrl.defaultZone=http://peer2:1112/eureka/
  
  #application-peer2.properties
  spring.application.name=eureka-server
  server.port=1112
  eureka.instance.hostname=peer2
  eureka.client.serviceUrl.defaultZone=http://peer1:1111/eureka/
  ```

- 在有多个eureka注册服务的情况下，服务提供者需要配置所有的eureka中心

  ```xml
  eureka.client.serviceUrl.defaultZone=http://peer1:1111/eureka/,http://peer2:1112/eureka/
  ```

- 服务提供者 可以有多个服务提供者 eureka会将所有的相同名称的服务，做成一个列表的形式，ribbon可以实现负载均衡 获取服务。

  ```XML
  spring.application.name=hello-service
  eureka.client.serviceUrl.defaultZone=http://localhost:1111/eureka/
  server.port=8082[8081]
  ```

- 服务消费者 通过@LoadBalanced来负载均衡

```java
@Bean
@LoadBalanced
RestTemplate restTemplate() {
    return RestTemplateMgr.getInstance().init().getTemplate();
}
```

- 服务续约

  ```xml
  #发送心跳个server的频率  默认30秒
  eureka.instance.lease-renewal-interval-in-seconds=30
  #两个心跳之间的时间间隔  超过则将服务摘除。 默认90秒
  eureka.instance.lease-expiration-duration-in-seconds=90
  
  ```

- 其他的一些配置 如：服务失效剔除、eureka的自我保护、服务下线等。

  ```xml
  # 单机调试的时候 可以关闭保护机制
  eureka.server.enable-self-preservation=false
  ```

- eureka服务的具体配置信息  可以查看：com.netflix.eureka.EurekaServerConfig 都是以eureka.server开头。

- eureka客户端的具体配置信息 可以查看：org.springframework.cloud.netflix.eureka.EurekaClientConfigBean  都是以eureka.client开头。

- 服务实例类配置信息 可以查看：org.springframework.cloud.netflix.eureka.EurekaInstanceConfigBean 都是以eureka.instance开头。

### 1.Eureka客户端

服务发现是microservice基础架构的关键原则之一。Eureka是Netflix服务发现的一种服务和客户端。

#### 1.1 注册到Eureka

当一个客户端注册到Eureka,它提供关于自己的元数据（诸如主机和端口，健康指标URL，首页等）Eureka通过一个服务从各个实例接收心跳信息。如果心跳接收失败超过配置的时间，实例将会正常从注册里面移除。

可以使用`@EnableEurekaClient`或`@EnableDiscoveryClient`注解。
配置`eureka.client.serviceUrl.defaultZone=http://localhost:8761/eureka/`属性，注意修改URL地址。
确保`spring.application.name`有默认值。

*注*：`@EnableEurekaClient`只能用于Eureka，`@EnableDiscoveryClient`能用于zookeeper等其他注册组件。

`@EnableEurekaClient`使其成为Eureka实例（被其他服务发现）和客户端（能发现其他服务）注册到应用里面。可通过`eureka.instance.*`进行相关配置的修改。

#### 1.2 对Eureka服务的身份验证

如果其中一个`eureka.client.serviceUrl.defaultZone`的url已经把凭证嵌入到它里面，那么HTTP基本的身份验证将会被自动添加到你的eureka客户端（curl风格，如`http://user:password@localhost:8761/eureka`）。 对于更复杂的需求，可以创建一个带“@Bean”注解的“DiscoveryClientOptionalArgs”类型并且为它注入“ClientFilter”实例。

#### 1.3 健康指标和状态页面

健康指标和状态页面分别对应一个Eureka实例的“/health”和“/info”。

#### 1.4 注册一个安全应用

若要用HTTPS，需要设置两个标记，分别是`EurekaInstanceConfig`，即`eureka.instance.[nonSecurePortEnabled,securePortEnabled]=[false,true]`。

#### 1.5 Eureka 健康检查

默认情况下，Eureka使用客户端心跳来确定一个客户端是否活着（状态为“UP”）。

#### 1.6 Eureka给客户端和实例的元数据

有标准的元数据，如主机名、IP地址、端口号、状态页面和健康检查。额外的元数据可以被添加到实例注册在`eureka.instance.metadataMap`里面。

**在 Cloudfoundry 使用 Eureka**
Cloudfoundry有总的路由，所有在同个应用的实例有相同的主机名。

**在AWS上使用Eureka**
定制`EurekaInstanceConfigBean`

**修改Eureka实例ID**
通过`eureka.instance.instanceId`进行配置。

#### 1.7 使用EurekaClient

使用`@EnableDiscoveryClient`（或`@EnableEurekaClient`），使它从Eureka Server发现服务实例。

注：不要在`@PostConstruct`方法或`@Scheduled`方法使用EurekaClient（或任何ApplicationContext还没被启动的地方）。

#### 1.8 代替原生的Netflix EurekaClient

Spring Cloud已经支持Feign（一个REST客户端构建）跟Spring RestTemplate（使用逻辑Eureka服务标识符(VIPs)代替物理的URL）。

#### 1.9 为什么注册一个服务这么慢？

实例默认与注册中心持续30秒的周期心跳（通过客户的`serviceUrl`）。当实例、服务端和客户端全都在它们的缓存里面拥有相同的元数据（这可能还需要3次心跳），那么服务对于客户端的discovery将变为不可用。可通过`eureka.instance.leaseRenewalIntervalInSeconds`这个修改配置，可以加快这一进程的客户端连接到其他服务。

### 2 服务发现: Eureka Server

例子，添加依赖`spring-cloud-starter-eureka-server`

```java
@SpringBootApplication
@EnableEurekaServer
public class Application {
    public static void main(String[] args) {
        new SpringApplicationBuilder(Application.class).web(true).run(args);
    }
}
```

服务端有一个带UI的首页，HTTP API端点在`/eureka/*`下提供标准的Eureka功能。

#### 2.1 高可用

Eureka服务端没有后台存储，但是服务实例在注册里面全都得发送心跳去保持注册更新（在内存里操作）客户端们也有一份内存缓存着eureka的注册信息。

#### 2.2 标准模式

```ym
eureka.client.registerWithEureka=false
eureka.client.fetchRegistry=false
eureka.client.serviceUrl.defaultZone=http://${eureka.instance.hostname}:${server.port}/eureka/
```

#### 2.3 节点感知

运行多个Eureka Servers。

Eureka甚至可以更有弹性和可用的运行多个实例，并让他们互相注册。事实上，这也是默认的行为，因此所有你需要让它工作的，只要添加一个有效的节点serviceUrl，例如：

```ym
---
spring:
  profiles: peer1
eureka:
  instance:
    hostname: peer1
  client:
    serviceUrl:
      defaultZone: http://peer2/eureka/

---
spring:
  profiles: peer2
eureka:
  instance:
    hostname: peer2
  client:
    serviceUrl:
      defaultZone: http://peer1/eureka/
```

然后通过不同的“profile”进行运行不同的Eureka Server。

#### 2.4 IP偏好

设置`eureka.instance.preferIpAddress=true`使应用注册到eureka的是IP地址而不是主机名。

## 三、Spring Cloud Config

**配置管理开发工具包，可以让你把配置放到远程服务器，目前支持本地存储、Git以及Subversion。**

针对系统外的配置项(如name-value对或相同功能的YAML内容),该服务器提供了基于资源的HTTP接口。使用**@EnableConfigServer**注解,该服务器可以很容易的被嵌入到Spring Boot 系统中。使用该注解之后该应用系统就是一个配置服务器。

```java
@SpringBootApplication
@EnableConfigServer
public class ConfigApplicion {
    public static void main(String[] args) throws Exception {
        SpringApplication.run(ConfigApplicion.class, args);
    }
}
```

### 1 资源库环境

- {application} 对应客户端的"spring.application.name"属性
- {profile} 对应客户端的 "spring.profiles.active"属性(逗号分隔的列表)
- {label} 对应服务端属性,这个属性能标示一组配置文件的版本

如果配置库是基于文件的，服务器将从application.yml和foo.yml中创建一个`Environment`对象。高优先级的配置优先转成`Environment`对象中的`PropertySource`。

#### 1 Git后端

默认的`EnvironmentRepository`是用Git后端进行实现的，Git后端对于管理升级和物理环境是很方便的,对审计配置变更也很方便。也可以`file:`前缀从本地配置库中读取数据。

这个配置库的实现通过映射HTTP资源的`{label}`参数作为git label(提交id,分支名称或tag)。如果git分支或tag的名称包含一个斜杠 ("/"),此时HTTP URL中的label需要使用特殊字符串"(_)"来替代(为了避免与其他URL路径相互混淆)。如果使用了命令行客户端如 curl，请谨慎处理URL中的括号(例如：在shell下请使用引号''来转义它们)。

**Git URI占位符**
Spring Cloud Config Server支持git库URL中包含针对{application}和 {profile}的占位符(如果你需要,{label}也可包含占位符, 不过要牢记的是任何情况下label只指git的label)。所以，你可以很容易的支持“一个应用系统一个配置库”策略或“一个profile一个配置库”策略。

**模式匹配和多资源库**

```ym
spring:
cloud:
    config:
      server:
        git:
          uri: https://github.com/spring-cloud-samples/config-repo
          repos:
            simple: https://github.com/simple/config-repo
            special:
              pattern: special*/dev*,*special*/dev*
              uri: https://github.com/special/config-repo
            local:
              pattern: local*
              uri: file:/home/configsvc/config-repo
```

如果 {application}/{profile}不能匹配任何表达式，那么将使用“spring.cloud.config.server.git.uri”对应的值。在上例子中，对于 "simple" 配置库, 匹配模式是simple/* (也就说,无论profile是什么，它只匹配application名称为“simple”的应用系统)。“local”库匹配所有application名称以“local”开头任何应用系统，不管profiles是什么(来实现覆盖因没有配置对profile的匹配规则，“/*”后缀会被自动的增加到任何的匹配表达式中)。

**Git搜索路径中的占位符**
`spring.cloud.config.server.git.searchPaths`

###### 1.2 版本控制后端文件系统使用

伴随着版本控制系统作为后端(git、svn)，文件都会被`check out`或`clone` 到本地文件系统中。默认这些文件会被放置到以config-repo-为前缀的系统临时目录中。在Linux上，譬如应该是`/tmp/config-repo-<randomid>`目录。有些操作系统[routinely clean out](https://serverfault.com/questions/377348/when-does-tmp-get-cleared/377349#377349)放到临时目录中，这会导致不可预知的问题出现。为了避免这个问题，通过设置`spring.cloud.config.server.git.basedir`或`spring.cloud.config.server.svn.basedir`参数值为非系统临时目录。

###### 1.3 文件系统后端

使用本地加载配置文件。
需要配置：`spring.cloud.config.server.native.searchLocations`跟`spring.profiles.active=native`。
路径配置格式：`classpath:/, classpath:/config,file:./, file:./config`。

###### 1.4 共享配置给所有应用

**基于文件的资源库**
在基于文件的资源库中(i.e. git, svn and native)，这样的文件名`application*`命名的资源在所有的客户端都是共享的(如 application.properties, application.yml, application-*.properties,etc.)。

**属性覆盖**
“spring.cloud.config.server.overrides”添加一个Map类型的name-value对来实现覆盖。
例如

```ym
spring:
  cloud:
    config:
      server:
        overrides:
          foo: bar
```

会使所有的配置客户端应用程序读取foo=bar到他们自己配置参数中。

#### 2. 健康指示器

通过这个指示器能够检查已经配置的`EnvironmentRepository`是否正常运行。
通过设置`spring.cloud.config.server.health.enabled=false`参数来禁用健康指示器。

#### 3 安全

你可以自由选择任何你觉得合理的方式来保护你的Config Server（从物理网络安全到OAuth2 令牌），同时使用Spring Security和Spring Boot 能使你做更多其他有用的事情。

为了使用默认的Spring Boot HTTP Basic 安全，只需要把Spring Security 增加到classpath中(如org.springframework.boot.spring-boot-starter-security)。默认的用户名是“user”，对应的会生成一个随机密码，这种情况在实际使用中并没有意义，一般建议配置一个密码（通过 security.user.password属性进行配置）并对这个密码进行加密。

#### 4 加密与解密

如果远程属性包含加密内容(以{cipher}开头),这些值将在通过HTTP传递到客户端之前被解密。

使用略

#### 5 密钥管理

配置服务可以使用对称(共享)密钥或者非对称密钥(RSA密钥对)。

### 2 可替换格式服务

配置文件可加后缀".yml"、".yaml"、".properties"

### 3 文本解释服务

```
/{name}/{profile}/{label}/{path}
```

### 4 嵌入配置服务器

一般配置服务运行在单独的应用里面，只要使用注解`@EnableConfigServer`即可嵌入到其他应用。

### 5 推送通知和总线

添加依赖`spring-cloud-config-monitor`，激活Spring Cloud 总线，`/monitor`端点即可用。

当webhook激活，针对应用程序可能已经变化了的，配置服务端将发送一个`RefreshRemoteApplicationEvent`。

### 6 客户端配置

#### 6.1 配置第一次引导

通过`spring.cloud.config.uri`属性配置Config Server地址

#### 6.2 发现第一次引导

如果用的是Netflix，则用`eureka.client.serviceUrl.defaultZone`进行配置。

#### 6.3 配置客户端快速失败

在一些例子里面，可能希望在没有连接配置服务端时直接启动失败。可通过`spring.cloud.config.failFast=true`进行配置。

#### 6.4 配置客户端重试

添加依赖`spring-retry`、`spring-boot-starter-aop`，设置`spring.cloud.config.failFast=true`。默认的是6次重试，初始补偿间隔是1000ms，后续补偿为1.1指数乘数，可通过`spring.cloud.config.retry.*`配置进行修改。

#### 6.5 定位远程配置资源

路径：`/{name}/{profile}/{label}`

- "name" = ${spring.application.name}
- "profile" = ${spring.profiles.active} (actually Environment.getActiveProfiles())
- "label" = "master"

label对于回滚到之前的版本很有用。

#### 6.6 安全

通过`spring.cloud.config.password`、`spring.cloud.config.username`进行配置。

## 四、springCloud Ribbon 客户端负载均衡

- RibbonEurekaAutoConfiguration 自动配置类
- 开启负载均衡的步骤：

1. 多个服务提供者，注册到服务中心
2. 服务消费者通过调用被@LoadBalanced注解修饰过的restTemplate

- RestTemplate 与 Ribbon整合使用 详解

1. RestTemplate 基本使用 GET POST PUT DELETE
2. RestTemplate 与 Ribbon 整合
3. 重点源码：LoadBalancerClient LoadBalancerAutoConfiguration

**Ribbon是一个客户端负载均衡器，有很多控制HTTP和TCP客户端的行为，可使用`@FeignClient`注解。**

```java
public class MyClass {
    @Autowired
    private LoadBalancerClient loadBalancer;

    public void doStuff() {
        ServiceInstance instance = loadBalancer.choose("stores");
        URI storesUri = URI.create(String.format("http://%s:%s", instance.getHost(), instance.getPort()));
        // ... do something with the URI
    }
}
```

## 五、Zuul API 网关服务

Zuul是Netflix出品的一个基于JVM路由和服务端的负载均衡器。

- 认证
- Insights
- 压力测试
- 金丝雀测试（Canary Testing）
- 动态路由
- 服务迁移
- 负载削减
- 安全
- 静态响应处理
- 主动/主动交换管理

### 1.Zuul的使用

```java
#在pom.xml引入spring-cloud-starter-zuul
#在application.properties配置
spring.application.name=api-gateway
server.port=5555
#在启动类使用@EnableZuulProxy
```

Spring Cloud创建了一个嵌入式Zuul代理来缓和急需一个UI应用程序来代理调用一个或多个后端服务的通用需求，这个功能对于代理前端需要访问的后端服务非常有用，避免了所有后端服务需要关心管理CORS和认证的问题。 

在Spring Boot主函数上通过注解`@EnableZuulProxy`来开启。

### 2.Zuul的主要功能有：

1. 请求转发，即路由的功能；与服务治理框架结合，实现自动化的服务实例维护以及负载均衡的路由转发。
2. 请求过滤，即可以当做是权限验证。权限校验与微服务业务逻辑解耦。
3. 它作为系统的统一入口，屏蔽了系统内部各个服务的细节。

### 3.传统路由方式

```xml
zuul.routes.api-a-url.path=/api-a-url/**
zuul.routes.api-a-url.url=http://localhost:8001/
```

### 4.面向服务的路由 使用eureka服务

```java 
zuul.routes.api-a.path=/api-a/**
zuul.routes.api-a.serviceId=hello-service

zuul.routes.api-b.path=/api-b/**
zuul.routes.api-b.serviceId=hello-service

zuul.routes.api-c.path=/ddd/**
zuul.routes.api-c.serviceId=hello-service

eureka.client.serviceUrl.defaultZone=http://localhost:1111/eureka/
```

使用`@EnableZuulProxy`同时引入了Spring Boot Actuator，默认将增加一个endpoint，提供http服务的`/routes`。

### 5.使用服务名称的方式 不用eureka服务治理

```xml
zuul.routes.api-d.path=/ddd/**
zuul.routes.api-d.serviceId=hello
ribbon.eureka.enabled=false
hello.ribbon.listOfServers=http://localhost:8001/,http://localhost:8002/
```

### 6.Cookie与头信息 为保证请求经过zuul转发后，还保留有Cookie Heads等信息，需要做一些配置：

```xml
#通过设置全局参数为空来覆盖默认值
zuul.senstitiveHeaders=
#通过制定路由的参数来配置  有如下两种配置
zuul.routes.<router>.customSensitiveHeaders=true
zuul.routes.<router>.senstiveHeaders=
```

### 7.重定向问题 spring security 和 shiro

```xml
zuul.addHostHeader=true
```

SpringCloud 的 Brixton会有重定向问题 Camden Dalston 则没有

- Hystrix和Ribbon支持 尽量使用 path与serviceId对应 即使用面向服务的路由。
- 动态加载/动态路由：原理 将配置放置在git远程仓库，更新仓库的配置文件，调用refresh接口，加载新的配置信息。
- 动态过滤器：使用groovy实现。

**窒息模式和本地跳转**

逐步替代旧的接口是一种通用的迁移现有应用程序或者API的方式，使用不同的具体实现逐步替换它们。Zuul代理是一种很有用的工具，因为你可以使用这种方式处理所有客户端到旧接口的请求。只是重定向了一些请求到新的接口。

## 六、springCloud Hystrix 服务容错保护

- HystrixCommand：用在依赖的服务返回单个操作结果的时候

- HystrixObservableCommand：用在依赖的服务返回多个操作结果的时候

- 通过几个注解的方式可以简单使用断路器的功能

  ```java
  #程序启动的地方
  @EnableCircuitBreaker 
  #具体需要断路器的服务方法上
  @HystrixCommand(fallbackMethod = "helloFallback", commandKey = "helloKey")
  #断路器被触发熔断的回调方法
  public String helloFallback() {}
  ```

Netflix创建了一个库实现断路器模式，名为Hystrix。一个低水平的服务群中一个服务挂掉会给用户导致级联失效的。当调用一个特定的服务达到一定阈值(在Hystrix里默认是5秒内20个失败)，断路器开启并且调用没有成功的。开发人员能够提供错误原因和开启一个断路由回调。

例子，需添加依赖`org.springframework.cloud.spring-cloud-starter-hystrix`

```
@SpringBootApplication
@EnableCircuitBreaker
public class Application {

    public static void main(String[] args) {
        new SpringApplicationBuilder(Application.class).web(true).run(args);
    }

}

@Component
public class StoreIntegration {

    @HystrixCommand(fallbackMethod = "defaultStores")
    public Object getStores(Map<String, Object> parameters) {
        //do stuff that might fail
    }

    public Object defaultStores(Map<String, Object> parameters) {
        return /* something useful */;
    }
}
```

经测试，失败就直接调用defaultStores()方法了。

### 1 传播安全上下文或使用 Spring Scopes

如果你想一些线程的本地的上下文传播到一个@HystrixCommand，默认声明将不会工作，因为他在线程池里执行命令。（在超时的情况下）。你可以切换Hystrix去使用同个线程让调用者使用一些配置，或直接在注解中，让它去使用不同的“隔离策略”。举例:

```
@HystrixCommand(fallbackMethod = "stubMyService",
    commandProperties = {
      @HystrixProperty(name="execution.isolation.strategy", value="SEMAPHORE")
    }
)
...
```

如果你使用`@SessionScope`或`@RequestScope`同样适用。你会知道你要这么做，因为一个runtime异常说他不能找到作用域上下文。

### 2 健康指标

连接的断路器的状态也暴露在应用程序的`/health`端点中。

### 3 Hystrix 指标流

添加依赖`org.springframework.boot.spring-boot-starter-actuator`，使`/hystrix.stream`流作为一个管理端点。

### 4 断路器: Hystrix仪表盘

Hystrix的主要作用是会采集每一个HystrixCommand的信息指标,把每一个断路器的信息指标显示的Hystrix仪表盘上。

![img](D:\Typora\笔记\image\46ae.png)

环境需要

- 添加依赖`org.springframework.cloud.spring-cloud-starter-hystrix-dashboard`
- 主类上注解`@EnableHystrixDashboard`
- 访问`/hystrix`
- 填写链接`http://host:port/hystrix.stream`

## 七、springCloud Sleuth 分布式服务跟踪

- 整合使用
- **日志收集工具包，封装了Dapper,Zipkin和HTrace操作。**

1. 添加pom.xml的依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-sleuth</artifactId>
</dependency>
```

1. 启动eureka服务
2. 访问  查看日志   分析日志即可看出链路调用的规则。  [trace-1,7cbdce82c9447510,7667724d864b3ec,false]

```tx
trace-1:应用名称  即application.properties中的spring.application.name的值
7cbdce82c9447510:springCloud Sleuth生成的一个ID  称为TraceID 用来标识一条请求链路
7667724d864b3ec:springCloud Sleuth生成的另外一个ID  称为SpanID 表示一个基本的工作单元 比如发送一个HTTP请求。
false:代表该信息是否要被后续的跟踪信息收集器获取和存储。

一条请求链路中  只能包含一个TraceID 可以有多SpanID
```

1. 服务跟踪的实现原理

```java
1.服务框架为每个请求创建唯一的跟踪标识。 一般是在httpHeader里标识
2.统计各个处理单元的时间耗时。 下一个单元开始 上一个单元结束。
3.源码跟踪：org.springframework.cloud.sleuth.Span
public static final String SAMPLED_NAME = "X-B3-Sampled";
public static final String PROCESS_ID_NAME = "X-Process-Id";
public static final String PARENT_ID_NAME = "X-B3-ParentSpanId";
public static final String TRACE_ID_NAME = "X-B3-TraceId";
public static final String SPAN_NAME_NAME = "X-Span-Name";
public static final String SPAN_ID_NAME = "X-B3-SpanId";
public static final String SPAN_EXPORT_NAME = "X-Span-Export";
```

- Sleuth抽样收集策略

```java
1. 通过Sampler接口实现  默认使用PercentageBasedSampler
    @Bean
    public AlwaysSampler defaultSampler() {
        return new AlwaysSampler();
    }
2. 通过配置文件配置
spring.sleuth.sampler.percentage=0.1  #代表获取10%的样例  1代表100%
```

- 与Logstash整合 收集日志分析

1. ELK平台  ElasticSearch/Logstash/Kibana这三个工具。
2. 配置logstash对JSON格式日志的支持

- 与Zipkin整合  **生产开发中，应该使用这个 附带有图形界面查看链路调用的服务。** 

1. 四大核心组件：

```
1. Collector：收集器组件，主要处理从外部系统发送过来的信息，将这些信息转换成Zipkin内部处理的Span格式，以支持后续的存储、分析、展示等功能。
2. Storage：存储组件，主要处理收集器收到的信息，默认会将这些信息存储在内存中。可以修改存储的策略，通过使用其他存储组件，将跟踪信息存储到数据库中。
3. Restful API：API组件，童工外部访问接口。
4. WEB UI：UI组件，基于API组件实现的上层应用。方便用户查询、分析跟踪信息。
```

1. Sleuth与Zipkin整合  HTTP方式。

```xml
1. 搭建ZipKinServer 服务
    <dependencies>
        <dependency>
            <groupId>io.zipkin.java</groupId>
            <artifactId>zipkin-server</artifactId>
        </dependency>
        <dependency>
            <groupId>io.zipkin.java</groupId>
            <artifactId>zipkin-autoconfigure-ui</artifactId>
        </dependency>
    </dependencies>
2. 为应用引入和配置ZipKin服务
    spring.zipkin.base-url=http://localhost:9411
```

1. Sleuth与Zipkin整合  消息中间件收集

```xml
1. 为具体应用添加pom依赖：
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-sleuth-stream</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-stream-rabbit</artifactId>
    </dependency>
2. 配置rabbitmq服务：
    spring.rabbitmq.host=localhost
    spring.rabbitmq.port=5672
    spring.rabbitmq.username=springcloud
    spring.rabbitmq.password=123456
3. 修改zipkin-server的pom依赖：
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-sleuth-zipkin-stream</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-stream-rabbit</artifactId>
    </dependency>
    <dependency>
        <groupId>io.zipkin.java</groupId>
        <artifactId>zipkin-autoconfigure-ui</artifactId>
    </dependency>
```

## 八、springCloud Feign 声明是服务调用

Feign 是一个声明web服务客户端，这便得编写web服务客户端更容易，使用Feign 创建一个接口并对它进行注解，它具有可插拔的注解:

```tx
支持包括Feign注解与JAX-RS注解

Feign还支持可插拔的编码器与解码器
```

Spring Cloud 增加了对 Spring MVC的注解，Spring Web 默认使用了HttpMessageConverters, Spring Cloud 集成 Ribbon 和 Eureka 提供的负载均衡的HTTP客户端 Feign.

Feign包含了Ribbon和Hystrix，所谓的包含并不是Feign的jar包包含有Ribbon和Hystrix的jar包这种物理上的包含，而是Feign的功能包含了其他两者的功能这种逻辑上的包含。

简言之：Feign能干Ribbon和Hystrix的事情，但是要用Ribbon和Hystrix自带的注解必须要引入相应的jar包才可以。



在应用主类中通过@EnableFeignClients注解开启Feign功能

启动文件FeignApplication.java

```java
@SpringBootApplication
@EnableDiscoveryClient
@EnableFeignClients
public class FeignApplication {   
	public static void main(String[] args) {        		 				                 SpringApplication.run(FeignApplication.class, args);   
	}
}
```

**其实通过Feign封装了HTTP调用服务方法，使得客户端像调用本地方法那样直接调用方法，类似Dubbo中暴露远程服务的方式，区别在于Dubbo是基于私有二进制协议，而Feign本质上还是个HTTP客户端**



**优点与缺点**
 使用`spring cloud feign`的继承特性的

	优点很明显，可以将接口的定义从`Controller`中剥离，同时配合maven仓库就可以轻易实现接口定义的共享，实现在构建期的接口绑定，从而有效的减少服务客户端的绑定配置。这么做虽然可以很方便的实现接口定义和依赖的共享，不用在复制粘贴接口进行绑定，但是这样的做法使用不当的话会带来副作用。
	
	由于接口在构建期间就建立起了依赖，那么接口变化就会对项目构建造成了影响，可能服务提供方修改一个接口定义，那么会直接导致客户端工程的构建失败。所以，如果开发团队通过此方法来实现接口共享的话，建议在开发评审期间严格遵守面向对象的开闭原则，尽可能低做好前后版本兼容，防止因为版本原因造成接口定义的不一致。