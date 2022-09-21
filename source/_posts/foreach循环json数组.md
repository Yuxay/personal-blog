---
title: foreach循环json数组
date: 2019-04-10 08:54:26
tags: js
categories: js
keywords: 'js,JavaScript'
aplayer: true
---

定义一组数据如下

```javascript
var obj = [
  {
    name: "a",
    age: 1,
  },
  {
    name: "b",
    age: 2,
  },
  {
    name: "c",
    age: 3,
  },
];
```

## 传入一个参数

```javascript
obj.forEach(function (element) {
  console.log(element);
});
```

![一个参数的结果](https://img-blog.csdnimg.cn/20190410084858131.png)
**可见：一个参数时，foreach 循环的是 json 数组中的每个对象**

## 传入两个参数

```javascript
obj.forEach(function (element, index) {
  console.log(element);
  console.log(index);
});
```

![两个参数结果](https://img-blog.csdnimg.cn/20190410085402490.png)
**可见：两个参数时，第一个参数是 json 数组中的对象，第二个参数是对应的 index 值**
