中文网： http://nodejs.cn/
node.js是javascript的运行环境，可用做服务端的开发，*但我至用于辅助前端开发*

>查看版本： node -v

# `node` 执行程序 
```bash
node script.js
```

# `npm` 包管理器
```bash
npm install
npm install <package-name>
# `--save` 安装并添加条目到 `package.json` 文件的 dependencies
# `--save-dev` 安装并添加条目到 `package.json` 文件的 devDependencies
# -g 默认将软件包安装到当前文件树的node_modules子文件夹下，使用-g会安装到全局位置

npm root -g #查看全局的node_modules位置
npm config ls -l #查看所有配置项
npm config edit #打开配置文件，可编辑
npm config set xxx xxx #设置配置

npm update
npm update <package-name>

npm run <task-name>

npm run watch
npm run dev
npm run prod

#查看过时的软件包
npm outdated
```

#  `ncu` npm包更新工具
```bash
npm install -g npm-check-updates # 安装到全局
ncu # 查看可更新的软件包
ncu -u # 更新当前文件树的packagejson的软件包，至最新版本
```