合适单页面、后台管理系统、不打开新标签页面的网站使用

# app-id方式
在chrome://apps可以查看应用页面，还可以自动生成图标
1. Chrome 打开某个网页，如： https://.www.pokill.com
2. 右上角，更多工具，创建快捷方式
3. 在窗口打开，会新启一个窗口

# app方式
就是一个网页的快速访问方式，不会在chrome://apps中展示，好处是可以粘贴复制给别人使用（前提是chrome的安装路径一致）
1. 右击，新建，快捷方式
2. 对象位置，"C:\Program Files (x86)\Google\Chrome\Application\chrome_proxy.exe" --profile-directory=Default --app=https://www.pokill.com
3. 会在一个独立窗口打开

# 区别
app-id有后退、刷新、网页翻译、更多，这些按钮
app则没有，但是可以使用快捷键
