---
title: vue打印echarts
date: 2019-04-12 18:18:56
tags: 
    - vue
    - echarts
categories: vue
keywords: 'vue,echarts'
aplayer: true
---
**思路**
将整个body的内容替换为弹窗的内容，打印操作完成之后还原
echarts打印时未显示，通过将其转为img的方法进行打印
```javascript
 //打印触发的方法
    print() {
      var that = this;
      var oldstr = document.body.innerHTML; // 获取当前页面内容用以还原
      var div_print = document.getElementById("printTest"); // 获取要打印部分的内容
      var cv = document.getElementsByTagName("canvas")[0]; //获取canvas
      var resImg = document.getElementsByClassName("resImg")[0]; //获取包裹canvas的标签
      // 将canvas转为图片
      var context = cv.getContext("2d");
      var img = new Image();
      var strDataURI = cv.toDataURL("image/png");
      img.src = strDataURI;
      // 图片加载完成之后
      img.onload = function() {
        // 执行打印
        console.log(img);
        setTimeout(function() {
          resImg.innerHTML = `<img src="${strDataURI}">`; // 用图片替代canvas
          var newstr = div_print.innerHTML;
          document.body.innerHTML = newstr; // 将页面内容改为修改后的内容
          window.print(); // 打印
          window.location.reload(); // 重新加载页面
          document.body.innerHTML = oldstr; // 将页面内容还原
        }, 1000);
      };
    },
```
