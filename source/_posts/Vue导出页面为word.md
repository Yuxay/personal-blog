---
title: Vue导出页面为word
date: 2019-05-11 10:27:14
tags: vue
categories: vue
keywords: 'vue'
aplayer: true
---
#### 由于导出word时，页面样式无法正常使用，因此整体页面采用table布局，仅在几个地方添加style样式，没有其余样式
```javascript
		  /**
			* 当页面中有canvas时，我的做法是
			* 在页面中预先放置一个src为空的img标签
			* 点击导出时，将canvas转为base64，将之前设置的img标签的src修改为base64，同时置空canvas
			* 需要注意的是，这种情况下，当结束导出操作时，由于页面中的canvas已经替换为图片，进行获取canvas标签操作时会报错
			* 因此需要进行判断
			* 当页面中存在canvas标签时，进行转换canvas操作
			* 否则直接导出页面内容
			*/
		  var cv = document.getElementsByTagName("canvas")[0]; //获取canvas
     	  var resImg = document.getElementsByClassName("resImg")[0]; //获取包裹canvas的标签
     	  var cimg = document.getElementsByClassName("cimg")[0];//获取空src的img标签
      	  // 将canvas转为图片
      	  var context = cv.getContext("2d");
      	  var srcDataURL = cv.toDataURL("image/png");
          cimg.setAttribute("src",srcDataURL);
          resImg.innerHTML = "";
          let tableConts =document.getElementsByClassName("tablepart")[0].innerHTML;// 获取要导出部分的内容
          
         
          // 将内容补完为完整的html页面
          let html_ = `<!DOCTYPE html>
                <html>
                <head>
                    <meta charset="utf-8">
                    <meta name="viewport" content="width=device-width,initial-scale=1.0">
                    <link rel="stylesheet" href="https://cdn.bootcss.com/iview/2.14.0/styles/iview.css" />
                    <style>
                    </style>
                </head>
                <body>
                    <div class="resume_preview_page" style="margin:0 auto;width:1200px">
                   ${tableConts}
                    </div>
                </body>
                </html>`;
    		// 将html页面发送至后台进行处理
            this.$axios({
              method: "post",
              data: html_,
              headers: {
              // 后台要求设置为json,使用时依据后台情况决定
                "Content-Type": "application/json"
              },

              responseType: "blob", //这里如果不设置，下载会打不开文件
              url: "http://192.168.0.125:9006/adminQuestion/exportWord"
            })
            .then(res => {
              //通过后台返回 的word文件流设置文件名并下载
              var blob = new Blob([res.data], {
                type:
                  "application/vnd.openxmlformats-officedocument.wordprocessingml.document;charset=utf-8"
              }); //application/vnd.openxmlformats-officedocument.wordprocessingml.document这里表示doc类型
              console.log(blob);
              var downloadElement = document.createElement("a");
              var href = window.URL.createObjectURL(blob); //创建下载的链接
              downloadElement.href = href;
              downloadElement.download = "a.doc"; //下载后文件名
              document.body.appendChild(downloadElement);
              downloadElement.click(); //点击下载
              document.body.removeChild(downloadElement); //下载完成移除元素
              window.URL.revokeObjectURL(href); //释放掉blob对象
            })
            .catch(err => {
              console.log(err);
            });
```
## 引用
[vue导出html、word和pdf](https://blog.csdn.net/weixin_34289454/article/details/88768689)
