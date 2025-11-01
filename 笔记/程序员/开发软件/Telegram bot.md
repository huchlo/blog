开发文档首页：[https://core.telegram.org/api](https://core.telegram.org/api)
各语言的开发库： https://core.telegram.org/bots/samples
tg的api文档采用纯接口请求的方式：`https://api.telegram.org/bot<token>/METHOD_NAME`
python可使用库 https://aiogram.dev/ 库中的方法也是对应官方的api的
例如： https://api.telegram.org/bot7544986494:AAFtDtKZcOZQVHcq1mNWD7665vsxHfS8-f4/getMe 返回机器人消息
https://api.telegram.org/bot7544986494:AAFtDtKZcOZQVHcq1mNWD7665vsxHfS8-f4/sendMessage?chat_id=7376772179&text=123 就是给7376772179发消息123

两种更新方式： **getUpdates** is a pull mechanism, **setWebhook** is push，setWebhook是tg后台主动推，getUpdates是用户主动拉

mini app 中获取用户信息，一定要是再mini app中，window.Telegram.WebApp.initDataUnsafe

```json
{
    "user": {
        "id": 2200844638,
        "first_name": "test",
        "last_name": "h",
        "language_code": "zh-hans",
        "allows_write_to_pm": true,
        "photo_url": "https://a-ttgme.stel.com/i/userpic/320/KUW1VVAG4y4DkyV_inBdcYKunch3Hq_QxGWxPnUzwlHkWX1-g540POkKSwLQgLqJ.svg"
    },
    "chat_instance": "-3477489894682795730",
    "chat_type": "private",
    "auth_date": "1736844315",
    "signature": "aDB9cKpxxOugxUL6QD58IUNx6JYr_yXBVlgCcMnuAeXIBd0k-3j0cVEryXCfmzSiIQ9HAXsCozHfFuNcTCIOAQ",
    "hash": "69efccb389f81cc864ee6e9c4ac260b8764e9ef9d78ccd01dfe763c48b1d9908"
}
```

# Bot Payments API
api文档 Payments 相关  [https://core.telegram.org/bots/api#payments](https://core.telegram.org/bots/api#payments)
Payments 文档 [https://core.telegram.org/bots/payments](https://core.telegram.org/bots/payments)
测试支付的时候，需要的虚拟账户列表： https://docs.stripe.com/testing#cards

## Telegram Stars
[https://core.telegram.org/bots/payments-stars](https://core.telegram.org/bots/payments-stars)

# mini app 测试环境
进入测试模式：[https://core.telegram.org/bots/webapps#testing-mini-apps](https://core.telegram.org/bots/webapps#testing-mini-apps)

	newbot
	newapp
	app的Web App URL设置为http://127.0.0.1
	web页head中插入
	`<script src="https://telegram.org/js/telegram-web-app.js?56"></script>`

telegram-web-app.js文档： [https://core.telegram.org/bots/webapps#initializing-mini-apps](https://core.telegram.org/bots/webapps#initializing-mini-apps)
小程序中拉起支付：请求自己的后台，后台使用createInvoiceLink返回支付的link数据，网页使用window.Telegram.WebApp.openTelegramLink，用户点击支付后还需要bot后台PreCheckoutQuery

# Widgets
[Telegram Widgets](https://core.telegram.org/widgets)
Sharing button 分享按钮
Post Widget  帖子预览
Login Widget  授权登录按钮
Discession Widget 帖子回复按钮
## Login Widget
web 登录按钮，打开新标签页需要输入手机号登录（个人觉得没什么用）： https://core.telegram.org/widgets/login 