后台接口数据body验证
1. @Validated 组过滤，
2. Json Schema验证
以上验证仅限于post请求

[Jackson使用手册 (altitude.xin)](https://www.altitude.xin/blog/home/#/chapter/60a4fe28746efc4e234e3724ece916c6?id=%F0%9F%A5%AD-jackson%E4%BD%BF%E7%94%A8%E6%89%8B%E5%86%8C)

https://www.injdk.cn/

xxx/em-sw/dev/master
xxx/config name/文件后缀/分支

# 自定义注解

- 请求拦截器获取注释
```java
@Target({ElementType.METHOD,ElementType.TYPE})  
@Retention(RetentionPolicy.RUNTIME)  
	public @interface UserLoginToken {  
}
```
```java
//Controller GETMapping @UserLoginToken
//MyInterceptor implements HandlerInterceptor preHandle
if (!(handler instanceof HandlerMethod)) {  
    return true;  
}  
HandlerMethod method = (HandlerMethod) handler;
UserLoginToken methodAnnotation = method.getMethodAnnotation(UserLoginToken.class);
UserLoginToken methodAnnotation  = method.getBeanType().getAnnotation(UserLoginToken.class);

//@Configuration public class WebMvcConfiguration implements WebMvcConfigurer
@Autowired  
private AuditLogInterceptor auditLogInterceptor;
@Override  
public void addInterceptors(InterceptorRegistry registry) {  
	registry.addInterceptor(myInterceptor).addPathPatterns("/**");
}


```




引入 org.springframework.web.servlet.HandlerInterceptor 接口
重写 preHandle postHandle afterCompletion 方法

# 远程调试
jdk8，服务运行时，加上

>-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005

意为启用5005端口提供远程调试服务
```bash
nohup java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005  -jar xxx.jar  >/dev/null 2>&1 &
```

本机创建 Remote JVM Debug，设置好host、module，port默认为5005，打好断点运行即可。

# 记录用户操作日志

```java
@Target({ElementType.METHOD})  
@Retention(RetentionPolicy.RUNTIME)
public @interface Log {  
    String value() default "";  
}
```
```java
@Component  
@Aspect  
public class LogAspect {

	@Pointcut("@annotation( com.config.Log )")  
	public void adminLogPoint() {  
	}  
	  
	@Around("adminLogPoint()")  
	public Object adminLogAround(ProceedingJoinPoint pjp) throws Throwable {  
	    Object msg = pjp.proceed();  
	    generateLog(pjp);  
	    return msg;  
	}

	private void generateLog(ProceedingJoinPoint pjp) {
		MethodSignature methodSignature = (MethodSignature) pjp.getSignature();  
		Object[] args = pjp.getArgs();
		Method method = methodSignature.getMethod();  
		Log log = method.<Log>getAnnotation(Log.class);  
		String logValue = log.value();
		//...
	}
}
```
```java
//使用，当调用这个方法时，会执行 generateLog 方法
@Log("登录")  
public ResponseEntity login() {  
    return null;  
}
```


# 启动参数调优
调优的目的，减少full gc 造成的应用程序停顿（stw，Stop一the一World）（停止业务线程，专心垃圾回收）

jvisualvm

jmap和jhat
jmap -histo:live pid
https://arthas.gitee.io/

## metaspace导致频繁FGC
可能原因：反射调用导致创建了大量的DelegatingClassLoader，占用了较大的元空间内存，内存碎片化，导致元空间利用率不高
调大meataspace，优化代码：不合理的反射

## CMS内存碎片化导致FGC
CMS使用标记清除算哒，不再进行压缩喝整理工作，意味着老年代随着应用的运行变得碎片化
业务低峰显式触发FGC
System.gc()
jmap -histo:live pid

## YFG和FGC频繁
扩大内存

## old gc耗时较长影响业务
可能原因：Remark阶段时间较长
-XX:CMSScavengeBeforeRemark
在重新标记前先执行一次新生代GC，因为老年代和年轻代之间的对象存在跨代引用，因此老年代进行GC Roots追踪时，同样也会扫描年轻代，而如果能够在重新标记前先执行一次新生代GC，那么就可以少扫描一些对象，重新标记阶段的性能也能因此提升。

## YGC耗时增加
jackson进行反序列化时，会将key进行String#intern，导致扫描时，GCRoot边打
禁用jackson的String#intern

## YGC次数增加
-XX:MaxGCPauserMillis参数世家设置过小
调大MaxGCPauserMillis值，将年轻代regin大小设置为固定值

# Mybatis
REQUIRED 必须的
```xml
<!ELEMENT select (#PCDATA | include | trim | where | set | foreach | choose | if | bind)*>
<!ATTLIST select
id CDATA #REQUIRED  唯一标识符
parameterMap CDATA #IMPLIED  弃用
parameterType CDATA #IMPLIED  传入参数的类
resultMap CDATA #IMPLIED  返回结果的映射
resultType CDATA #IMPLIED  返回结果的类，如果返回的是集合，则应设为集合包含的类型
resultSetType (FORWARD_ONLY | SCROLL_INSENSITIVE | SCROLL_SENSITIVE) #IMPLIED
statementType (STATEMENT|PREPARED|CALLABLE) #IMPLIED
fetchSize CDATA #IMPLIED
timeout CDATA #IMPLIED
flushCache (true|false) #IMPLIED
useCache (true|false) #IMPLIED 
databaseId CDATA #IMPLIED
lang CDATA #IMPLIED
resultOrdered (true|false) #IMPLIED
resultSets CDATA #IMPLIED
>
```
choose、when、otherwise
when就相当于if，otherwise就相当于else，但是when、otherwise需要放在choose里面才能使用

```xml
<choose> 
        <when test="list != null and list.size() > 0"> xxx </when>
        <otherwise>XXX</otherwise> 
</choose>
```


# 代码段
```java
//Java中遍历Map的两种方法：keySet和entrySet
Set<String> set = map.keySet();   
for (String s:set) {  
	System.out.println(s+","+map.get(s));  
}
Set<Map.Entry<String, String>> entryseSet=map.entrySet();  
for (Map.Entry<String, String> entry:entryseSet) {  
	System.out.println(entry.getKey()+","+entry.getValue());  
}
```

```java
//实例化线程
    public static void main(String[] args) {
        Thread tr1=new TestThread(); //实例化线程

        tr1.start(); //启动线程
        Thread tr2=new Thread(new TestRunable()); //实例化线程并传入线程体

        tr2.start(); //启动线程
        Thread tr3=new Thread(new TestThreadpool()); //实例化线程并传入线程体
        ExecutorService tp=Executors.newFixedThreadPool(10); //创建具有10个线程的线程池
        tp.execute(tr3);//启动线程
    }
    //使用Thread创建线并启动线程
    class TestThread extends Thread{
        public void run(){
            for(int i=0;i<10;i++){
                System.out.println("Thread>"+Thread.currentThread().getName()+":"+i);
            }
        }

    }
    //使用Runnable创建并启动线程
    class TestRunable implements Runnable{
        public void run(){
            for(int j=0;j<10;j++){
                System.out.println("Runnable>"+Thread.currentThread().getName()+":"+j);
            }
        }

    }
    //使用ExecutorService实现线程池
    class TestThreadpool implements Runnable{
        public void run(){
            for(int k=0;k<10;k++){
                System.out.println("ThreadPool>"+Thread.currentThread().getName()+":"+k);
            }
        }

    }
```

```java
    //匿名内部类方式实例化线程
    public static void main(String[] args) throws IOException {


       //使用Thread创建线并启动线程
        new Thread(){
        public void run(){
            while(true){
                System.out.println("1");
            }
        }
        }.start();

        //使用Runnable创建并启动线程
        new Thread(new Runnable(){
            public void run(){
                while(true){
                    System.out.println("2");
                }
            }
        }).start();

        //使用ExecutorService实现线程池
        Executors.newFixedThreadPool(10).execute(new Runnable() {
            @Override
            public void run() {
                while(true){
                    System.out.println("3");
                }
            }
        });
    }
```