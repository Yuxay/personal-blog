---
title: JS判断安卓，ios和微信
date: 2019-04-28 16:33:26
tags: js
categories: js
keywords: 'js,Javascript'
aplayer: true
---
```javascript
var u = navigator.userAgent;
var ua = navigator.userAgent.toLowerCase();
// 判断安卓
u.indexOf("Android") > -1 || u.indexOf("Linux") > -1;
// 判断iOS
!!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/); //ios终端
// 判断是否在微信内置浏览器打开
if(ua.match(/MicroMessenger/i) == "micromessenger"){
	alert('当前页面在微信中打开')
}
```
