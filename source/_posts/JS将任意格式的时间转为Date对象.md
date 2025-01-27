---
title: JS将任意格式的时间转为Date对象
date: 2022-08-24 14:47:04
tags: js
categories: js
keywords: 'js,Javascript'
aplayer: true
---
```js
/**
 * 将任意格式的日期转为 new Date() 类型
 * @param {*} date 日期
 * @param {boolean} allowNull 转换结果是否允许为null
 * @returns
*/
function convertAnyToDate(date, allowNull = false) {
      let dateType = Object.prototype.toString.call(date); // 传入的时间的类型
      let timeObj = null; // 时间对象
      // 获取时间对象
      if (dateType == "[object Date]") {
        timeObj = new Date(date);
      } else if (dateType == "[object String]") {
        // 判断是否为纯数字，纯数字即视为时间戳
        let test = /^\d+$/.test(date); 
        if (test) {
          let tempDate = parseInt(date);
          let tempTimeStamp = date.length == 10 ? tempDate * 1000 : tempDate;
          timeObj = new Date(tempTimeStamp);
        } else {
          // 利用是否能转换为时间戳判断是否为日期格式字符串
          let tempTime = new Date(date).getTime(); 
          if (null != tempTime && undefined != tempTime && !isNaN(tempTime)) {
            timeObj = new Date(tempTime);
          }
        }
      } else if (dateType == "[object Number]") {
          let timestamp = date.toString().length == 10 ? date * 1000 : date;
          timeObj = new Date(timestamp);
      }
      if (timeObj == null && !allowNull) {
        timeObj = new Date();
      }
      return timeObj;
    };
```