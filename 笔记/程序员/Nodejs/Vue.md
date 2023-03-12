中文官网： https://v3.cn.vuejs.org/
实例生命周期图示： https://v3.cn.vuejs.org/images/lifecycle.svg

# Vue CLI 
用于快速创建和开发vue项目
```
npm install -g @vue/cli #全局安装vue命令行

vue --version
vue create hello-world 
vue ui #使用可视化界面
```


- vsCode 插件
Vetur

- 文字一行
```css
text-overflow: ellipsis;
overflow: hidden;
white-space: nowrap;
width: 10px; //需要固定宽度
flex: 1 1 0%; //弹性布局也需要固定宽度
```

- 文字超出两行省略... 
```css
overflow: hidden;
text-overflow: ellipsis;
display: -webkit-box;
-webkit-line-clamp: 2;
-webkit-box-orient: vertical;
```

- 图片和文字水平
```html
<span style="vertical-align: middle;">
	<img/>
</span>
<span style="vertical-align: middle;">text</span>
```

- grid自适应最小宽度
```css
display: grid;
grid-template-rows: min-content repeat(2, auto);

```