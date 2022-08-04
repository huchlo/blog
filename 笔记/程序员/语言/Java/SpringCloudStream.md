# kafka
springboot中使用kafka可以选择两种方式
- 直接引包使用
简单快捷、但与系统高耦合，以后想改成另外的消息中间件就比较麻烦了
```xml
<dependency>  
    <groupId>org.springframework.kafka</groupId>  
    <artifactId>spring-kafka</artifactId>  
</dependency>

@KafkaListener
```
- 通过SpringCloudStream集成
解耦
```xml
<dependency>  
    <groupId>org.springframework.cloud</groupId>  
    <artifactId>spring-cloud-stream</artifactId>  
</dependency>
<dependency>  
    <groupId>org.springframework.cloud</groupId>  
    <artifactId>spring-cloud-starter-stream-kafka</artifactId>  
</dependency>

@EnableBinding
@StreamListener
```
[SpringCloudStream集成Kafka - 哔哩哔哩 (bilibili.com)](https://www.bilibili.com/read/cv16484789)

