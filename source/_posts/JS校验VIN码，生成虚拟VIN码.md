---
title: JS校验VIN码，生成虚拟VIN码
date: 2021-11-22 14:44:11
tags: js
categories: js
keywords: 'js,Javascript'
aplayer: true
---
### 定义需要使用到的一些常量
```js
/** VIN码允许使用的字符数组 */
const CharArray = ['1', '2', '3', '4', '5', '6', '7', '8', 'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'J', 'K', 'L', 'M', 'N', 'P', 'R', 'S', 'T', 'V', 'W', 'X', 'Y'];
/** 国标规定的VIN码的前两位数组 */
const ChinaArray = ['L0','L1','L2', 'L3', 'L4', 'L5', 'L6', 'L7', 'L8', 'L9', 'LA', 'LB', 'LC', 'LD', 'LE', 'LF', 'LG', 'LH', 'LJ', 'LK', 'LL', 'LM', 'LN', 'LP', 'LR', 'LS', 'LT', 'LU', 'LV', 'LW', 'LX', 'LY', 'LZ', 'H0', 'H1', 'H2', 'H3', 'H4', 'H5', 'H6', 'H7', 'H8', 'H9', 'HA', 'HB', 'HC', 'HD', 'HE', 'HF', 'HG', 'HH', 'HJ', 'HK', 'HL', 'HM', 'HN', 'HP', 'HR', 'HS', 'HT', 'HU', 'HV', 'HW', 'HX', 'HY', 'HZ',];
/** VIN码中各位置对应的加权值数组 */
const WeightValue = [8, 7, 6, 5, 4, 3, 2, 10, 0, 9, 8, 7, 6, 5, 4, 3, 2];
```

### 定义CharArray中的字符对应的值
```js
/** CharArray中的字符对应的值 */
const VerifyArr = setVerify();

/** 将VIN中各码对应的值存入MAP中 */
function setVerfiy(){
  let map = new Map();
  // VIN码中各字母的charcode值
  let charCodeArr = [65, 66, 67, 68, 69, 70, 71, 72, 74, 75, 76, 77, 78, 80, 82, 83, 84, 85, 86, 87, 88, 89, 90];
  // VIN码中各对应的值
  let valueArr = [1, 2, 3, 4, 5, 6, 7, 8, 1, 2, 3, 4, 5, 7, 9, 2, 3, 4, 5, 6, 7, 8, 9];
  // 以字母为key值，存储该字母对应的值
  for (let i = 0; i < 23; i++) {
    map.set(String.fromCharCode(charCodeArr[i]), valueArr[i]);
  }
  // 以数字('字符串化')为key值，存储该数字对应的值
  for (let j = 0; j < 10; j++) {
    map.set(j + '', j);
  }
  // 抛出最终结果
  return map;
}
```

### 验证VIN码中第九位是否正确
```js
/** 计算VIN码的校验位 */
function getVerifyCode(vin) {
  // VIN码从从第一位开始，码数字的对应值×该位的加权值，计算全部17位的乘积值相加除以11，所得的余数，即为第九位校验值
  let tempVinArr = vin.split('');
  let tempRes = 0;
  for (let i = 0; i < 17; i++) {
    tempRes += VerifyArr.get(tempVinArr[i] + '') * WeightValue[i];
  }
  let OperationRes = tempRes % 11;
  let res = '';
  if (OperationRes != 10) {
    res = OperationRes + '';
  } else {
    res = 'X';
  }
  return res;
}

/** 判断VIN是否正确 */
function isCorrectVin(vin) {
  let verifyCode = getVerifyCode(vin);
  if (vin.substring(8, 9) == verifyCode) {
    return true;
  } else {
    return false;
  }
}
```
### 生成随机的虚拟VIN码
```js
/** 生成随机前缀(前五位) */
function setRandomBeforeStr() {
  let res = getRandomChar(ChinaArray);
  res += getRandomChar(CharArray);
  for (let i = 0; i < 5; i++) {
    res += getRandomChar(CharArray);
  }
  return res;
}
/** 生成随机后缀(10-12位) */
function setRandomAfterStr() {
  let res = '';
  for (let i = 0; i < 3; i++) {
    res += getRandomChar(CharArray);
  }
  res += productNo();
  return res;
}

/** 生成随机的生产序号 */
function productNo() {
  var result = '';
  for (var i = 0; i < 5; i++) {
    result += Math.floor(Math.random() * 16).toString(16); //获取0-16并通过toString转16进制
  }
  //默认字母小写，手动转大写
  return result.toUpperCase(); //另toLowerCase()转小写
}
/** 拼接车架号 */
function spellVin(beforeStr, afterStr) {
  let preVin = beforeStr + 'X' + afterStr;
  let verifyCode = getVerifyCode(preVin);
  let vin = beforeStr + verifyCode + afterStr;
  if (isCorrectVin(vin)) {
    return vin;
  } else {
    spellVin(beforeStr, afterStr);
  }
}

/** 返回随机字符 */
function getRandomChar(array) {
  return array[parseInt((Math.random() * 100) % array.length)];
}

/** 生成虚拟VIN码 */
function getRandomVin() {
    let beforeStr = setRandomBeforeStr();
    let afterStr = setRandomAfterStr();
    let vin = spellVin(beforeStr, afterStr);
    return vin;
}
```