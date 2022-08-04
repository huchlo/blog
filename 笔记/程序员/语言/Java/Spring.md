Component、Service、Controller、Repository

- @Component
>最普通，标记为bean
- @Controller
>标记为bean并导入到 IOC 容器，配合@RestController，@RequestMapping、@@GetMapping等
- @Service
>作用同Component，就是名字不一样，用于注解业务逻辑层
- @Repository，持久层
>标记为bean，一般用于注解Dao，可以将标注类抛出的异常转换为转换为 Spring 的数据访问异常类型 （Spring DataAccessException）