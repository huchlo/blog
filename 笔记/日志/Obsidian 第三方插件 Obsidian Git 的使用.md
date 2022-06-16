Obsidian Git 版本：1.25.3
- 配置git环境
- 生成公钥
  >ssh-keygen -t rsa -C "xxx@xxx.com"
- 将.pub的内容复制到gitee设置公钥的位置
- 此后，使用ssh方式clone代码就可以不输入账号密码了
- 检验设置是否成功
  >git clone git@xxx.git
- Custom Git binary path 默认为git，如果没有设置git的环境变量需要指定git.exe的位置
> 如：D:\Git\bin\git