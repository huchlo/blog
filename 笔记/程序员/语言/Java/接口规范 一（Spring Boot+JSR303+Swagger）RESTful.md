[Spring Boot实现参数校验_亭子下的李子的博客-CSDN博客](https://blog.csdn.net/weixin_43407520/article/details/118480962)


```java
@ExceptionHandler(Exception.class)  
public JsonResult handleException(Exception e) {  
    logger.error("catch an undefined exception.", e);  
    return ReturnUtil.fail(ErrorConstant.COMMON_SERVER_ERROR);  
}
```

# JSR-303

<table>
	<tr>
		<td>分类</td>
		<td>注解</td>
		<td>说明</td>
	</tr>
	<tr>
		<td rowspan="4">空和非空检查</td>
		<td>@Null</td>
		<td>只能为 null</td>
	</tr>
	<tr>
		<td>@NotNull</td>
		<td>必须不为 null</td>
	</tr>
	<tr>
		<td>@NotEmpty</td>
		<td>不为 null 且不为空</td>
	</tr>
	<tr>
		<td>@NotBlank</td>
		<td>不为 null 且去除首位空格长度为0</td>
	</tr>
	<tr>
		<td rowspan="2">Boolean值检查</td>
		<td>@AssertFalse</td>
		<td>必须为false</td>
	</tr>
	<tr>
		<td>@AssertTrue</td>
		<td>必须为true</td>
	</tr>
	<tr>
		<td>长度检查</td>
		<td>@Size(max,min)</td>
		<td>字符长度必须在min到max之间</td>
	</tr>
	<tr>
		<td rowspan="4">日期检查</td>
		<td>@Future</td>
		<td>必须为之后时间</td>
	</tr>
	<tr>
		<td>@FutureOrPresent</td>
		<td>必须为当前时间或之后</td>
	</tr>
	<tr>
		<td>@Past</td>
		<td>必须为之前时间</td>
	</tr>
	<tr>
		<td>@PastOrPresent</td>
		<td>必须为当前时间或之前</td>
	</tr>
	<tr>
		<td rowspan="9">数值检查</td>
		<td>@Max(value)</td>
		<td>必须lt;=value</td>
	</tr>
	<tr>
		<td>@Min(value)</td>
		<td>必须>=value</td>
	</tr>
	<tr>
		<td>@DecimalMax(value)</td>
		<td>必须&lt;=value</td>
	</tr>
	<tr>
		<td>@DecimalMin(value)</td>
		<td>必须>=value</td>
	</tr>
	<tr>
		<td>@Digits(integer,fraction)</td>
		<td>必须为一个小数，且整数部分位数不能超过integer，小数部分的位数不能超过fraction</td>
	</tr>
	<tr>
		<td>@Negative</td>
		<td>必须&lt;0</td>
	</tr>
	<tr>
		<td>@NegativeOrZero</td>
		<td>必须&lt;=0</td>
	</tr>
	<tr>
		<td>@Positive</td>
		<td>必须>0</td>
	</tr>
	<tr>
		<td>@PositiveOrZero</td>
		<td>必须>=0</td>
	</tr>
	<tr>
		<td rowspan="2">其他值检查</td>
		<td>@Pattern(regexp)</td>
		<td>必须符合指定的正则表达式</td>
	</tr>
	<tr>
		<td>@Email</td>
		<td>必须为email格式</td>
	</tr>
</table>


注意
1、内部类反序列化问题
使之变成静态内部类
2、@NOtNull不能注释原始类型成员
如使用@NotNull注释int类型参数，须使用Integer，否则不生效

## Valid和@Validated 区别
Valid没有分组功能，可以用在成员属性上
Validated有分组功能，不能用在成员属性上，其他一样
# 异常处理
```java
//示例
@ExceptionHandler(Exception.class)  
public String handleException(Exception e) {   
    return "出错啦~";  
}
```

处理请求类型错误异常： `HttpRequestMethodNotSupportedException`
数据格式不对:： `HttpMessageNotReadableException`
参数校验异常： `MethodArgumentNotValidException`
自定义异常： `MyException`
处理其他代码中未捕获未转换的异常： `Exception`

