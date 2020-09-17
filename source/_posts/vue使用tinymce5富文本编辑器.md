---
title: vue使用tinymce5富文本编辑器
date: 2019-08-12 09:59:52
tags: 
    - vue
    - tinymce
    - 富文本编辑器
categories: vue
cover: https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3963915081,2771048512&fm=26&gp=0.jpg
keywords: 'vue,tinymce,富文本编辑器'
aplayer: true
---
**极力推荐一个大佬做的[tinymce5中文文档](http://tinymce.ax-z.cn/)；其中包含部分常用插件如：多图上传，行间距，首行缩进等**
<hr>

***我所使用的版本为***
```javascript
"@tinymce/tinymce-vue": "^2.1.0",
"tinymce":"^5.0.12"
```
通过npm 安装tinymce和tinymce-vue
```javascript
npm install tinymce -S
npm install @tinymce/tinymce-vue -S
```
# 一、子组件页面

## 1.首先需要引入tinymce的关键文件
```javascript
import tinymce from "tinymce";
import "tinymce/themes/silver";
import Editor from "@tinymce/tinymce-vue";
```
## 2.此时的tinymce包含了一些基本功能插件，如果需要其他功能，需要引入对应的功能插件，并在plugins和toolbar中使用插件
```javascript
import "tinymce/themes/silver";
import "tinymce/plugins/paste";
import "tinymce/plugins/image";
import "tinymce/plugins/link";
import "tinymce/plugins/code";
import "tinymce/plugins/table";
import "tinymce/plugins/lists";
import "tinymce/plugins/wordcount";
```
## 3.使用tinymceVue组件
```html
<template>
  <div class="tinymce-editor">
    <editor
      v-model="myValue"
      :init="init"
    ></editor>
  </div>
</template>
<script>
	export default {
		components: {
	    	Editor
  		},
	}
</script>
```
## 4.用props接收父级页面传来的value值，通过watch进行value的修改
```javascript
props: {
    value: {
      type: String,
      default: ""
    },
    plugins: {
      type: [String, Array],
      default:
        "link lists image code table wordcounts"
    },
    toolbar: {
      type: [String, Array],
      default:
        "bold italic underline strikethrough | fontsizeselect fontselect | forecolor backcolor | alignleft aligncenter alignright alignjustify | bullist numlist | outdent indent blockquote | undo redo | link unlink code | removeformat"
    }
  },
watch: {
    value(newValue) {
      this.myValue = newValue;
    },
    myValue(newValue) {
      this.$emit("input", newValue);
    }
  }
```
## 5.声明所需要的参数
```javascript
data: function() {
    return {
      myValue: this.value,
    };
  },
```
## 6.在data中进行编辑器相关的配置
```javascript
data function() {
	return {
		init: {
        language_url: "/static/plugins/tinymce/zh_CN.js", //如果语言包不存在，指定一个语言包路径
        language: "zh_CN", //语言
        skin_url: "/static/plugins/tinymce/skins/ui/oxide", //如果主题不存在，指定一个主题路径
        content_css: "/static/plugins/tinymce/mycontent.css",
        height: "700px",
        width: this.calcWidth(),
        plugins: this.plugins, //插件
        toolbar: this.toolbar, //工具栏
        branding: false, //技术支持(Powered by Tiny || 由Tiny驱动)
        menubar: true, //菜单栏
        theme: "silver", //主题
        zIndex: 1101,
      },
	}
}
```
## 7.可以在methods中定义一些常用的方法
```javascript
methods: {
	// 使编辑器的宽度始终为页面的一半
	calcWidth() {
      return document.body.clientWidth / 2 + "px";
    },
}
```
# 完整子组件代码
```html
<template>
  <div class="tinymce-editor">
    <editor
      v-model="myValue"
      :init="init"
      :disabled="disabled"
      @onClick="onClick"
    ></editor>
  </div>
</template>
<script>
import axios from "axios";
import tinymce from "tinymce";
import Editor from "@tinymce/tinymce-vue";
import "tinymce/themes/silver";
import "tinymce/plugins/paste";
import "tinymce/plugins/image";
import "tinymce/plugins/link";
import "tinymce/plugins/code";
import "tinymce/plugins/table";
import "tinymce/plugins/lists";
import "tinymce/plugins/wordcount";

export default {
  components: {
    Editor
  },
  props: {
    value: {
      type: String,
      default: ""
    },
    disabled: {
      type: Boolean,
      default: false
    },
    plugins: {
      type: [String, Array],
      default:
        "link lists image code table wordcounts"
    },
    toolbar: {
      type: [String, Array],
      default:
        "bold italic underline strikethrough | fontsizeselect fontselect | forecolor backcolor | alignleft aligncenter alignright alignjustify | bullist numlist | outdent indent blockquote | undo redo | link unlink code | removeformat"
    }
  },
  data() {
    return {
      init: {
        language_url: "/static/plugins/tinymce/zh_CN.js", //如果语言包不存在，指定一个语言包路径
        language: "zh_CN", //语言
        skin_url: "/static/plugins/tinymce/skins/ui/oxide", //如果主题不存在，指定一个主题路径
        content_css: "/static/plugins/tinymce/mycontent.css",
        height: "700px",
        width: this.calcWidth(),
        plugins: this.plugins, //插件
        toolbar: this.toolbar, //工具栏
        branding: false, //技术支持(Powered by Tiny || 由Tiny驱动)
        menubar: true, //菜单栏
        theme: "silver", //主题
        zIndex: 1101,
      },
      myValue: this.value
    };
  },
  mounted() {
    tinymce.init({});
  },
  methods: {
    calcWidth() {
      return document.body.clientWidth / 2 + "px";
    },
  },
  watch: {
    value(newValue) {
      this.myValue = newValue;
    },
    myValue(newValue) {
      this.$emit("input", newValue);
    }
  }
};
</script>

```
# 二、在父级页面使用
```html
<template>
	 <editor-vue class="editor" :value="dataForm.lessonDetail" @input="(res)=> dataForm.lessonDetail = res" ></editor-vue>
</template>
<script>
import editorVue from "../../../components/editor.vue";
export default{
	components:{editorVue},
}
</script>
```