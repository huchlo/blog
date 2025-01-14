开发文档首页：[Telegram APIs](https://core.telegram.org/api)


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
js文档 Payments 相关  [Telegram Bot API](https://core.telegram.org/bots/api#payments)
Payments 文档 [Bot Payments API](https://core.telegram.org/bots/payments)
## Telegram Stars
[Bot Payments API for Digital Goods and Services](https://core.telegram.org/bots/payments-stars)

# mini app 测试环境
进入测试模式：[Telegram Mini Apps](https://core.telegram.org/bots/webapps#testing-mini-apps)
newbot
newapp
app的Web App URL设置为http://127.0.0.1
web页head中插入
`<script src="https://telegram.org/js/telegram-web-app.js?56"></script>`

telegram-web-app.js文档： [Telegram Mini Apps](https://core.telegram.org/bots/webapps#initializing-mini-apps)

# Widgets
[Telegram Widgets](https://core.telegram.org/widgets)
Sharing button 分享按钮
Post Widget  帖子预览
Login Widget  授权登录按钮
Discession Widget 帖子回复按钮
## Login Widget
web 登录按钮，打开新标签页需要输入手机号登录（个人觉得没什么用）： https://core.telegram.org/widgets/login 