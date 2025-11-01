#EPay 商户接口文档

##Md5 签名算法

> 1、所有参数按键对数组(key1=value1)进行升序排序(参数值为空时不参与签名)  
> 2、按照 key1=value1&key2=value2 进行组合  
> 3、加上商户秘钥 key1=value1&key2=value2...&key=商户秘钥  
> 3、sign=md5(上一步组装待签名字的符串) 签名结果 32 位小写  
> 4、签名密钥,请在商户后台->基础信息 查看,或者询问我方客服

##注意事项  
 本文档中的所有接口，均采用 HTTP 标准通信协议，请求和响应的 Content-type 均为 application/json，字符编码统一为 UTF-8。
金额单位为分。
请求接口的 IP 需要加白。

#代收下单接口  
(下单 ip 需要联系我方加白)  
下单地址 https://order.8ae.one/payApi/PayApi/CreateOrder

| 名称           | 类型   | 必填  | 描述                        |
| -------------- | ------ | ----- | --------------------------- |
| trade_no       | int    | true  | 商户号                      |
| app_id         | int    | true  | 商户 appId                  |
| pay_code       | int    | true  | 产品编码,联系我方运营获取   |
| price          | int    | true  | 下单金额,单位:分 ,整数      |
| order_no       | string | true  | 商户订单号                  |
| success_url    | string | false | 支付成功跳转 url            |
| fail_url       | string | false | 支付失败跳转 url            |
| pay_notice_url | string | false | 支付成功通知 url            |
| user_id        | string | false | 用户 id                     |
| user_ip        | string | false | 付款人 IP                   |
| attach         | string | false | 附加参数                    |
| sign           | string | true  | 签名结果,签名方法在文档顶部 |

请求参数示例

```json
{
  "trade_no": 165,
  "order_no": "p71584121t1693047656571",
  "app_id": 51,
  "pay_code": 91145,
  "price": 10000,
  "success_url": "",
  "fail_url": "",
  "pay_notice_url": "http://www.test.com",
  "pay_remind_url": "",
  "attach": "",
  "user_ip": "87.200.59.188",
  "user_id": "2677323",
  "sign": "db3406277185f9660b3b928d6adc7bc4"
}
```

##下单响应

| 名称         | 类型   | 必填 | 描述                        |
| ------------ | ------ | ---- | --------------------------- |
| code         | int    | true | 200:下单成功 其他:下单失败  |
| msg          | string | true | 失败原因                    |
| pay_url      | string | true | 付款链接                    |
| qr_code      | string | true | 二维码字符串                |
| order_no     | string | true | 商户订单号                  |
| dis_order_no | string | true | 平台订单号                  |
| create_time  | int    | true | 创建时间                    |
| sign         | string | true | 签名结果,签名方法在文档顶部 |

###响应示例  
失败:

````json
{
	"code": 10009,
	"msg": "ip不在商户ip白名单, ip:158.28.87.267",
	"sign": "db3406277185f9660b3b928d6adc7bc4",
	"pay_url": "",
	"qr_code": "",
	"order_no": "",
	"dis_order_no": "",
	"create_time":0
}`

成功:
```json
{
	"code": 200,
	"msg": "success",
	"pay_url": "https://fourpay-intest2.ncjimmy.com/payApi/PayApi/CallBack/3_97091602d4501dbffce038ca6419e176",
	"qr_code": "TNVPdaYdXnUxADTfLcKvkam4836B2vkKT5",
	"order_no": "47210116924681604173",
	"dis_order_no": "lufei169246816001692",
	"sign": "db3406277185f9660b3b928d6adc7bc4",
	"create_time":1695317066
}
````

#代收回调通知  
推送地址:商户下单传送的 pay_notice_url 回调 ip:43.199.22.57,18.162.85.201,16.162.155.123 ,请将我方 ip 加入回调白名单

| 名称           | 类型     | 必填    | 描述               |
| ------------ | ------ | ----- | ---------------- |
| trade_no     | int    | true  | 商户号              |
| status       | int    | true  | 订单状态, 2.成功, 3.失败 |
| order_no     | string | true  | 商户订单号            |
| dis_order_no | string | true  | 平台订单号            |
| order_price  | int    | true  | 订单金额,单位:分        |
| real_price   | int    | true  | 用户真实付款金额 ,单位:分   |
| fee          | int    | false | 订单手续费 ,单位:分      |
| nti_time     | int    | false | 发起通知时间           |
| payer        | string | false | JSON 字符串,付款人信息   |
| attach       | string | false | 附加参数             |
| create_time  | int    | true  | 创建时间             |
| sign         | string | true  | 签名结果,签名方法在文档顶部   |

###请求参数示例

```json
{
  "trade_no": 165,
  "status": 2,
  "order_no": "p71584121t1693047656571",
  "dis_order_no": "lufei169246816001692",
  "order_price": 10000,
  "real_price": 10000,
  "fee": 10,
  "nti_time": 1693057443,
  "payer": "{\"name\":\"姓名\",\"email\":\"邮箱\",\"phone\":\"手机号\"}",
  "attach": "",
  "create_time": 1695317066,
  "sign": "db3406277185f9660b3b928d6adc7bc4"
}
```

回调接收处理成功，请返回 success，系统将不再推送此订单信息，否则还会重复推送多次

#代付下单接口  
(下单 ip 需要联系我方加白)  
下单地址 https://order.8ae.one/payApi/PayApi/CreatePayOutOrder

| 名称             | 类型     | 必填    | 描述             |
| -------------- | ------ | ----- | -------------- |
| trade_no       | int    | true  | 商户号            |
| order_no       | string | true  | 商户订单号          |
| app_id         | int    | true  | 商户 appId       |
| pay_code       | int    | true  | 产品编码,联系我方运营获取  |
| price          | int    | true  | 下单金额,单位:分 ,整数  |
| account_no     | string | true  | 出款账号           |
| account_type   | string | true  | 账号类型:BANK,UPI  |
| account_name   | string | true  | 姓名             |
| bank_code      | string | true  | 固定值： INDIAN    |
| identify_num   | string | true  | IFSC号码         |
| pay_notice_url | string | false | 代付成功通知 url     |
| attach         | string | false | 附加参数           |
| sign           | string | true  | 签名结果,签名方法在文档顶部 |

###请求参数示例

```json
{
  "trade_no": 165,
  "order_no": "p71584121t1693047656571",
  "app_id": 51,
  "pay_code": 91145,
  "price": 10000,
  "account_type": "EMAIL",
  "account_no": "asda12321@gmail.com",
  "account_name": "TEST",
  "bank_code": "PIX",
  "identify_num": "10703569562",
  "pay_notice_url": "http://www.test.com",
  "attach": "",
  "sign": "db3406277185f9660b3b928d6adc7bc4"
}
```

##下单响应

| 名称           | 类型     | 必填   | 描述                                    |
| ------------ | ------ | ---- | ------------------------------------- |
| code         | int    | true | 200:下单成功 其他:下单失败                      |
| msg          | string | true | 失败原因                                  |
| dis_order_no | string | true | 平台订单号                                 |
| order_no     | string | true | 商户订单号                                 |
| status       | int    | true | 订单状态 2.代付成功, 3.代付失败, 7.驳回 8.冲正,其他:代付中 |
| create_time  | int    | true | 创建时间                                  |
| sign         | string | true | 签名结果,签名方法在文档顶部                        |

###响应示例

失败:

```json
{
  "code": 10009,
  "msg": "下单失败"
}
```

成功:

```json
{
  "code": 200,
  "msg": "success",
  "status": 1,
  "order_no": "47210116924681604173",
  "dis_order_no": "lufei169246816001692",
  "create_time": 1695317066,
  "sign": "db3406277185f9660b3b928d6adc7bc4"
}
```

#代付回调通知  
推送地址:商户下单传送的 pay_notice_url 回调 ip:43.199.22.57,18.162.85.201,16.162.155.123 ,请将我方 ip 加入回调白名单

| 名称           | 类型     | 必填    | 描述                              |
| ------------ | ------ | ----- | ------------------------------- |
| trade_no     | int    | true  | 商户号                             |
| order_no     | string | true  | 商户订单号                           |
| dis_order_no | string | true  | 平台订单号                           |
| price        | int    | true  | 订单金额,单位:分                       |
| fee          | int    | false | 订单手续费 ,单位:分                     |
| status       | int    | true  | 订单状态, 2.代付成功, 3.代付失败, 7.驳回 8.冲正 |
| attach       | string | false | 附加参数                            |
| remark       | string | false | 失败原因                            |
| create_time  | int    | true  | 创建时间                            |
| sign         | string | true  | 签名结果,签名方法在文档顶部                  |

##请求参数示例

```json
{
  "trade_no": 165,
  "status": 2,
  "order_no": "p71584121t1693047656571",
  "dis_order_no": "lufei169246816001692",
  "order_price": 10000,
  "real_price": 10000,
  "fee": 10,
  "attach": "",
  "remark": "",
  "create_time": 1695317066,
  "sign": "db3406277185f9660b3b928d6adc7bc4"
}
```

回调接收处理成功，请返回 success，系统将不再推送此订单信息，否则还会重复推送多次

#查询接口 (代收 代付共用)  
(请求 ip 需要联系我方加白)  
查询地址: https://order.8ae.one/payApi/PayApi/QueryOrder

| 名称       | 类型   | 必填 | 描述                        |
| ---------- | ------ | ---- | --------------------------- |
| order_type | string | true | pay_out:代付,pay_in:代收    |
| trade_no   | int    | true | 商户号                      |
| order_no   | string | true | 商户订单号                  |
| sign       | string | true | 签名结果,签名方法在文档顶部 |

###请求参数示例

```json
{
  "order_type": "pay_in",
  "trade_no": 165,
  "order_no": "p71584121t1693047656571",
  "sign": "db3406277185f9660b3b928d6adc7bc4"
}
```

##查询响应

| 名称           | 类型     | 必填    | 描述                                                     |
| ------------ | ------ | ----- | ------------------------------------------------------ |
| code         | int    | true  | 200:查询成功 其他:失败                                         |
| msg          | string | true  | 查询失败原因                                                 |
| trade_no     | int    | true  | 商户号                                                    |
| real_price   | int    | true  | 真实付款金额 ,单位:分                                           |
| status       | int    | true  | 订单状态:1.未支付, 2.成功, 3.失败    , 7.驳回 8.冲正                  |
| success_time | int    | true  | 成功时间戳                                                  |
| order_no     | string | true  | 商户订单号                                                  |
| dis_order_no | string | true  | 平台订单号                                                  |
| remark       | string | true  | 代付失败原因                                                 |
| fee          | int    | false | 订单手续费 ,单位:分                                            |
| create_time  | int    | true  | 创建时间                                                   |
| payer        | string | false | JSON 字符串,付款人信息{"name":"姓名","email":"邮箱","phone":"手机号"} |
| sign         | string | true  | 签名结果,签名方法在文档顶部                                         |

###响应示例

失败:

```json
{
  "code": 10009,
  "msg": "订单不存在"
}
```

成功:

```json
{
  "code": 200,
  "msg": "success",
  "trade_no": 123,
  "real_price": 10000,
  "status": 2,
  "success_time": 1693057443,
  "order_no": "47210116924681604173",
  "dis_order_no": "lufei169246816001692",
  "remark": "",
  "fee": 10,
  "create_time": 1695317066,
  "payer": "{\"name\":\"姓名\",\"email\":\"邮箱\",\"phone\":\"手机号\"}",
  "sign": "db3406277185f9660b3b928d6adc7bc4"
}
```

#余额查询接口  
(请求 ip 需要联系我方加白)  
地址: https://order.8ae.one/payApi/PayApi/Balance

| 名称     | 类型   | 必填 | 描述                        |
| -------- | ------ | ---- | --------------------------- |
| trade_no | int    | true | 商户号                      |
| app_id   | int    | true | appid                       |
| sign     | string | true | 签名结果,签名方法在文档顶部 |

###请求参数示例

```json
{
  "trade_no": 165,
  "app_id": 28,
  "sign": "db3406277185f9660b3b928d6adc7bc4"
}
```

##查询响应

| 名称    | 类型   | 必填 | 描述                        |
| ------- | ------ | ---- | --------------------------- |
| code    | int    | true | 200:查询成功 其他:失败      |
| msg     | string | true | 失败原因                    |
| balance | int    | true | 余额,单位:分                |
| sign    | string | true | 签名结果,签名方法在文档顶部 |

###响应示例

失败:

```json
{
  "code": 10001,
  "msg": "商户不存在"
}
```

成功:

```json
{
  "code": 200,
  "msg": "success",
  "balance": 10000,
  "sign": "db3406277185f9660b3b928d6adc7bc4"
}
```

#银行编码

```json
{
        "UPI":"UPI",
	"INDIAN":    "Indian Bank"	
}
```