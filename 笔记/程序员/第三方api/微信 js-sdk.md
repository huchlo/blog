微信 JS 接口签名校验工具:http://mp.weixin.qq.com/debug/cgi-bin/sandbox?t=jsapisign
js例子，查看附件，或https://my.oschina.net/superkangning/blog/367484
1、如何在openLocation，入参
```java
document.querySelector('.storeName').onclick = function () {
	var la=parseFloat($(this).attr("latitude"));//纬度
	var lo=parseFloat($(this).attr("longitude"));//经度
	var add={"latitude":la,"longitude":lo};
	alert( JSON.stringify(add));
	wx.openLocation({
	  latitude: add.latitude,
	  longitude: add.longitude,
	  name: 'TIT 创意园',
	  address: '广州市海珠区新港中路 397 号',
	  scale: 14,
	  infoUrl: 'http://weixin.qq.com'
	});
};
```
 parseFloat是js中，字符串转浮点数的方法

2、怎么识别是否是微信查看网页
```java
if (browser.versions.mobile) {//判断是否是移动设备打开。browser代码在下面
  var ua = navigator.userAgent.toLowerCase();//获取判断用的对象
  if (ua.match(/MicroMessenger/i) == "micromessenger") {
    //在微信中打开
  }
  if (ua.match(/WeiBo/i) == "weibo") {
    //在新浪微博客户端打开
  }
  if (ua.match(/QQ/i) == "qq") {
    //在QQ空间打开
  }
  if (browser.versions.ios) {
    //是否在IOS浏览器打开
  }
  if(browser.versions.android){
    //是否在安卓浏览器打开
  }
} else {
  //否则就是PC浏览器打开
}
 	var browser = {
 versions: function () {
  var u = navigator.userAgent, app = navigator.appVersion;
  return {   //移动终端浏览器版本信息
   trident: u.indexOf('Trident') > -1, //IE内核
   presto: u.indexOf('Presto') > -1, //opera内核
   webKit: u.indexOf('AppleWebKit') > -1, //苹果、谷歌内核
   gecko: u.indexOf('Gecko') > -1 && u.indexOf('KHTML') == -1, //火狐内核
   mobile: !!u.match(/AppleWebKit.*Mobile.*/), //是否为移动终端
   ios: !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/), //ios终端
   android: u.indexOf('Android') > -1 || u.indexOf('Linux') > -1, //android终端或uc浏览器
   iPhone: u.indexOf('iPhone') > -1, //是否为iPhone或者QQHD浏览器
   iPad: u.indexOf('iPad') > -1, //是否iPad
   webApp: u.indexOf('Safari') == -1 //是否web应该程序，没有头部与底部
  };
 }(),
 language: (navigator.browserLanguage || navigator.language).toLowerCase()
}
```




























