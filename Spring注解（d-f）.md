## Spring注解（d-f）

DependsOn：定义Bean初始化及销毁时的顺序

DateTimeFormat：加上此注解，可解析时间格式的字符串

Document：说明该注解将被包含在javadoc中

Documented：是否写入帮助文档中去。

DecimalMax：(value=值) 被标注的值必须不大于约束中指定的最大值. 这个约束的参数是一个通过BigDecimal定义的最大值的字符串表示.小数存在精度

DecimalMin：(value=值) 被标注的值必须不小于约束中指定的最小值. 这个约束的参数是一个通过BigDecimal定义的最小值的字符串表示.小数存在精度

Digits：验证 Number 和 String 的构成是否合法 

Digits：(integer=,fraction=)验证字符串是否是符合指定格式的数字，interger指定整数精度，fraction指定小数精度。

Email ：验证是否是邮件地址，如果为null,不进行验证，算通过验证。

EnableLoadTimeWeaving：使得加载时间织入

EnableMBeanExport：是经过@Import将JMX相关的bean界说加载到IoC容器

EnableLoadTimeWeaving：使得加载时间织入

EnableAsync：开始对异步任务的支持，并在相应的方法中使用@Async注解来声明一个异步任务。 

EnableScheduling：开启对计划任务的支持，然后在要执行计划任务的方法上注解@Scheduled，声明这是一个计划任务。

ExceptionHandler：统一处理某一类异常，从而能够减少代码重复率和复杂度

EnableAutoConfiguration也是凭借@Import的协助，将一切契合主动装备条件的bean界说加载到IoC容器。

EnableScheduling是经过@Import将Spring调度结构相关的bean界说都加载到IoC容器。

EnableAspectJAutoProxy：开启AOP,

EnableTransactionManagement：开启spring事务管理,

EnableCaching：开启spring缓存

EnableWebMvc： 开启webMvc

Filter：过滤器

Filter.annotation ：按照注解进行扫描

Filter.type ： 按照指定类进行扫描

Filter.custom ：自定义过滤器



