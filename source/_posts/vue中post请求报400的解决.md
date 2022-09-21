---
title: vue中post请求报400的解决
date: 2019-04-19 14:51:03
tags: 
    - vue
    - axios
categories: vue
keywords: 'vue,axios'
aplayer: true
---
1、为默认数据格式为json,发请求时参数报错
通过以下方式修改数据格式即可
```javascript
import qs from 'qs';
// import qs from 'querystring'
const data = { 'bar': 123 };
const options = {
  method: 'POST',
  headers: { 'content-type': 'application/x-www-form-urlencoded' },
  data: qs.stringify(data),
  url,
};
axios(options);
```
2、检查发送的数据格式是否与后端要求相匹配，要求字符串，发送了数组也可能会出现400错误
