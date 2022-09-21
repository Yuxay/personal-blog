---
title: 关于vue中el-date-picker type=daterange日期不回显的问题
date: 2019-06-13 10:33:47
tags: 
    - vue
    - element-ui
categories: vue
keywords: 'vue,element-ui'
aplayer: true
---


### 一、原始代码
```html
<el-form-item class="form_bigt_p" label="项目起止时间:" prop="time">
  <el-date-picker
    unlink-panels
    class="bigWidth"
    :disabled="isDisabled"
    v-model.trim="ruleForm.time"
    type="daterange"
    value-format="timestamp"
    range-separator="至"
    start-placeholder="开始日期"
    end-placeholder="结束日期"
  ></el-date-picker>
</el-form-item>
```

#### 由于后台返回的数据是两个 yyyy-mm-dd 格式的日期(startTime,endTime)，因此一开始采用

```javascript
this.ruleForm.time = [
  this.baseDateTemp(res.data.startTime),
  this.baseDateTemp(res.data.endTime),
];
//this.baseDateTemp是全局的转日期为时间戳的方法
```
<!-- more -->
### 二、问题发现及处理
#### 问题
得到的日期可以渲染在 el-date-picker 上，但是修改的时候不会回显
经测试后发现，此时可以触发 input 方法，但不触发 change 方法
#### 处理方式
在 input 方法中可知，修改时，el-date-picker 所绑定的 v-model 的值已经改变，但是控件中没有实时更新
最终选择采用 this.\$set 方法进行数据的更新，并成功解决此问题
修改后代码如下

```html
<el-form-item class="form_bigt_p" label="项目起止时间:" prop="time">
  <el-date-picker
    unlink-panels
    class="bigWidth"
    :disabled="isDisabled"
    v-model.trim="ruleForm.time"
    type="daterange"
    value-format="timestamp"
    range-separator="至"
    start-placeholder="开始日期"
    end-placeholder="结束日期"
    @input="testClick"
  ></el-date-picker>
</el-form-item>
```

```javascript
    testClick(e) {
      this.$nextTick(() => {
        this.ruleForm.time = null;
        this.$set(this.ruleForm, "time", [e[0], e[1]]);
      });
    },
    // 将后台获取到的两个日期转为time的方法修改为
        this.$set(self.ruleForm, "time", [
          this.baseDateTemp(res.data.startTime),
          this.baseDateTemp(res.data.endTime)
        ]);
```
