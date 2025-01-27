---
title: JS判断数据是否为空
date: 2022-03-22 14:46:09
tags: js
categories: js
keywords: 'js,Javascript'
aplayer: true
---
使用`Object.prototype.toString`判断数据的类型
```javascript
function isEmpty(val){
	let valType = Object.prototype.toString.call(val);
	let isEmpty = false;
	switch (valType) {
		case "[object Undefined]":
		case "[object Null]":
			isEmpty = true;
			break;
		case "[object Array]":
     	case "[object String]":
     		try {
        		isEmpty = val + "" === "null" || val + "" === "undefined" || val.length <= 0 ||  val.split("").length <= 0 ? true : false;
      		} catch (error) {
        		isEmpty = false;
      		};
      		break;
		case "[object Object]":
			try {
				let temp = JSON.stringify(val);
				isEmpty = temp + "" === "null" || temp + "" === "undefined" || temp === "{}" ? true : false;
			} catch (error) {
				isEmpty = false;
			}
			break;
		case "[object Number]":
			isEmpty = val + "" === "NaN" || val + "" === "Infinity" ? true : false;
			break;
		default:
			isEmpty = false;
			break;
	}
	return isEmpty;
}
```