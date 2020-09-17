---
title: 记一次vue+element项目中echarts的展示问题
date: 2019-04-12 18:15:40
tags: 
    - vue
    - element-ui
    - echarts
categories: vue
cover: https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=1020757406,2710278676&fm=26&gp=0.jpg
keywords: 'vue,element-ui,echarts'
aplayer: true
---
## 展示
**前提：**
后台返回的是一段绘制用的js脚本，并是以父向子传参的方法传递过来，无法以正常的import方式使用echarts
**解决：**
```javascript
// 通过此方法引入echarts所需js文件
methods:{
	introduce(){
		var script1 = document.createElement("script");
     	 script1.src = "http://www.chinaops.cn/cloud/home/js/01/jquery-1.4.2.min.js";
      	document.head.append(script1);
	}
},
created(){
	this.introduce();
}
```
```javascript
data(){
	return{
		script:null // 这是script标签
	}
},
props:{
	scr:"", // 这是传过来的js脚本
},
methods:{
	introduce(){
	  // 创建一个空的script标签用以存放js脚本
      this.script = document.createElement("script");
      document.body.append(this.script);
	}
},
computed(){
	draw(){
		// 将js脚本放入预先创建的script标签
		this.script.innerHTML = this.scr;
	}
}
```
```html
<template>
 	<div class="resImg">
          {{draw}}<!-- 调用绘制方法 -->
          <div id="factors_image" name="factors_image" :style="{width: '300px', height: '300px'}"></div>
          <!-- 存放echarts的div   id,name为js脚本中所规定 -->
    </div>
</template>
```
```javascript
// 父组件中echarts数据的处理
 // 传递echarts所需数据
 get请求.then(res => {
		this.json = res.data.factorJson;
        // 传递echarts所需脚本
        var str = res.data.graphicsScript;
        str = this.insertStr(str, 0, "var factors =" + this.json + ";");
        this.scr = str;
})
/** 后台传递的数据都存放在res.data中  
*	factorJson是以键值对方式存放的echarts所需的数据
*	graphicsScript是以字符串方式存放的绘制echarts的方法
*	因为传过来的方法中无法取得数据，因此使用字符串拼接的方法将数据拼接进graphicsScript中
*/
 // 字符串拼接的方法
    insertStr(soure, start, newStr) {
      return soure.slice(0, start) + newStr + soure.slice(start);
    }
```
factorJson的数据
![factorJson](https://img-blog.csdnimg.cn/20190415144831818.png)
graphicsScript的数据
![graphicsScript](https://img-blog.csdnimg.cn/2019041514491444.png)