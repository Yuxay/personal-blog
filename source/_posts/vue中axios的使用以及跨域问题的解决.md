---
title: vue中axios的使用以及跨域问题的解决
date: 2019-04-09 15:46:39
tags: 
    - vue
    - axios
categories: vue
cover: https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=2774029514,3120630809&fm=26&gp=0.jpg
keywords: 'vue,axios'
aplayer: true
---
## 在vue中使用axios
 使用 npm install axios 安装
 在main.js中添加以下代码
```javascript
import Axios from "axios";
Vue.prototype.$axios = Axios;
```


## 解决跨域问题
 在config中的index.js修改ProxyTable为如下：
```javascript
proxyTable: {
      "/api": {
        target: "后台提供的路径前缀",
        changeOrigin: true,
        pathRewrite: {
          '^/api': ''
        }
      }
    },
```
<hr>

### 在某次使用时，出现修改后发生404的情况，解决方法如下：

修改proxyTable
```javascript
proxyTable: {
      "/api": {
        target: "后台提供的路径前缀",
        changeOrigin: true,
        pathRewrite: {
          '^/api': '/api'
        }
      }
    },
```
 在main.js中添加以下代码
```javascript
Axios.defaults.baseURL = '/api' // 使每次发送请求时带一个/api的前缀
```
 使用如下
```javascript
get请求
this.$axios.get("study/study-result"，{params:{key:value}}).then(res =>{
	console.log(res)
}).catch(err =>{
	console.log(err)
})

post请求
this.$axios.post("study/add-work",{key:value}).then(res =>{
	console.log(res)
}).catch(err =>{
	console.log(err)
})
```
