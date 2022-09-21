---
title: vue + tinymce上传图片到七牛以及图片批量上传
date: 2019-11-22 11:57:22
tags: 
    - vue
    - tinymce
    - 七牛云
    - 图片上传
categories: vue
keywords: 'vue,tinymce,七牛云,图片上传'
aplayer: true
---
###### 此文基于之前博文[vue使用tinymce5富文本编辑器](https://blog.csdn.net/qq_41926416/article/details/98199375)
###### 图片批量上传插件来自[Tinymce5 ax多图片批量上传插件](http://tinymce.ax-z.cn/more-plugins/axupimgs.php)
## 一、上传图片到七牛云
#### 1.安装图片上传以及图片批量上传插件
```javascript
import "tinymce/plugins/image";
import "tinymce/plugins/axupimgs";
```
#### 2.在toolbar和plugins中使用这两个插件,使用自定义图片上传方法
```javascript
plugins:"image axupimgs",
toolbar:"image axupimgs"
```
#### 3.在data中存入七牛云所需的数据
```javascript
data:function{
	return {
	  QiniuData: {
        key: "", //图片名字处理
        token: "" //七牛云token
      },
      domain: "http://upload.qiniup.com", // 七牛云的上传地址
      qiniuaddr: "http://img.example.com" // 七牛云的图片外链地址
	}
}
```
#### 4.获取token，自定义图片上传方法
```javascript
getQiniuToken() {
      this.$http({
        url: this.$http.adornUrl("/lesson/file/qiniu-token"),
        method: "get"
      })
        .then(res => {
          this.QiniuData.token = res.data.token;
        })
        .catch(error => {});
    },
    imgUpload(blobInfo, success, failure) {
      const axiosInstance = axios.create({ withCredentials: false }); //withCredentials 禁止携带cookie，带cookie在七牛上有可能出现跨域问题
      let data = new FormData();
      data.append("token", this.QiniuData.token); //七牛需要的token，叫后台给，是七牛账号密码等组成的hash
      data.append("file", blobInfo.blob());
      axiosInstance({
        method: "POST",
        url: this.domain, //上传地址
        data: data,
        timeout: 30000, //超时时间，因为图片上传有可能需要很久
        onUploadProgress: progressEvent => {
          //imgLoadPercent 是上传进度，可以用来添加进度条
          let imgLoadPercent = Math.round(
            (progressEvent.loaded * 100) / progressEvent.total
          );
        }
      })
        .then(data => {
		  // 调用成功回调，返回用七牛外链地址和返回的key拼接的图片路径
          success(`${this.qiniuaddr}/${data.data.key}`);
        })
        .catch(function(err) {
          //上传失败
        });
    }
```
#### 5.调用方法
```javascript
mounted:function(){
	this.getQiniuToken();
},
data(){
	init:{
		images_upload_handler: (blobInfo, success, failure) => {
          this.imgUpload(blobInfo, success, failure);
        }
	}
}
```
## 二、图片批量上传
#### 1.此插件基于image插件，仍旧调用之前的图片上传方法
#### 2.建议将此插件放入static或其他目录，修改plugin.js文件中的文件路径
```javascript
var baseURL=tinymce.baseURL;
var iframe1 = baseURL + '/static/plugins/axupimgs/upfiles.html';
```
