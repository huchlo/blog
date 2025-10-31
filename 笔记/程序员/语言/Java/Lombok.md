mlombok插件官网：https://projectlombok.org

使用lombok分两步
1. 在开发工具（如idea）安装lombok插件
2. 在项目中引入lombok包

```xml
<dependencies>
	<dependency>
		<groupId>org.projectlombok</groupId>
		<artifactId>lombok</artifactId>
		<version>1.18.24</version>
		<scope>provided</scope>
	</dependency>
</dependencies>
```

lombok稳定特性：https://projectlombok.org/features/all  
lombok实验特性：https://projectlombok.org/features/experimental/all

目前常用：
# @Data
https://projectlombok.org/features/Data  
作用于类，是@ToString、@EqualsAndHashCode、@Getter、@Setter、@RequiredArgsConstructor、@EqualsAndHashCode,、@Value的集合
- @Getter/@Setter https://projectlombok.org/features/GetterSetter
- @ToString https://projectlombok.org/features/ToString
- @EqualsAndHashCode https://projectlombok.org/features/EqualsAndHashCode
- @RequiredArgsConstructor https://projectlombok.org/features/constructor

# @Value
@Value是@Data的不可变用法，不包含@Setter，构造器会包含全部成员变量，成员变量会加上final
https://projectlombok.org/features/Value

# @NonNull
https://projectlombok.org/features/NonNull
作用于方法的参数上，执行方法时该入参为Null，则会抛出NullPointerException

# @Builder
https://projectlombok.org/features/Builder  
作用于类，将类转变为建造者模式
```java
Person.builder()
  .name("Adam Savage")
  .city("San Francisco")
  .job("Mythbusters")
  .job("Unchained Reaction")
  .build();
```

# @Synchronized
作用于静态或示例方法，与关键字`synchronized`的区别是，`synchronized`作用于this，@会为每个方法创建object锁

# @Getter(lazy=true)
https://projectlombok.org/features/GetterLazy  
单例模式

# @Log
作用于类上，生成日志变量，针对不同的日志实现产品，有不同的注解
```java
@CommonsLog
private static final org.apache.commons.logging.Log log = org.apache.commons.logging.LogFactory.getLog(LogExample.class);
@Flogger
private static final com.google.common.flogger.FluentLogger log = com.google.common.flogger.FluentLogger.forEnclosingClass();
@JBossLog
private static final org.jboss.logging.Logger log = org.jboss.logging.Logger.getLogger(LogExample.class);
@Log
private static final java.util.logging.Logger log = java.util.logging.Logger.getLogger(LogExample.class.getName());
@Log4j
private static final org.apache.log4j.Logger log = org.apache.log4j.Logger.getLogger(LogExample.class);
@Log4j2
private static final org.apache.logging.log4j.Logger log = org.apache.logging.log4j.LogManager.getLogger(LogExample.class);
@Slf4j
private static final org.slf4j.Logger log = org.slf4j.LoggerFactory.getLogger(LogExample.class);
@XSlf4j
private static final org.slf4j.ext.XLogger log = org.slf4j.ext.XLoggerFactory.getXLogger(LogExample.class);
@CustomLog
private static final com.foo.your.Logger log = com.foo.your.LoggerFactory.createYourLogger(LogExample.class);

```

# @Cleanup
自动关闭资源，针对实现了java.io.Closeable接口的对象有效，如：典型的IO流对象
```java
@Cleanup InputStream in = new FileInputStream(args[0]);  
@Cleanup OutputStream out = new FileOutputStream(args[1]);
```
