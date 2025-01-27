---
title: VUE2项目安装使用less
date: 2024-09-04 14:49:03
tags: 
    - vue
    - less
categories: vue
keywords: 'vue,less'
aplayer: true
---
编辑文章时所使用vue版本
```json
{
	"vue": "^2.5.2",
}
```
## 一、安装less、less-loader和sass-resources-loader
```bash
npm install less@3.0.4 less-loader@5.0.0 sass-resources-loader@2.2.5
```

## 二、修改`build/webpack.base.conf.js`配置
```javascript
moduel.exports = {
	...,
	module: {
		rules: [
			...,
			{
				test: /\.less$/,
				use: ['vue-style-loader', 'css-loader', 'less-loader']
			}
		]
	}
}
```
## 三、修改`build/util.js`配置
```javascript
// 修改已有的generateLoaders
function generateLoaders(loader, loaderOptions) {
	const loaders = options.usePostCSS ? [cssLoader, postcssLoader] : [cssLoader]

    if (loader) {
      if (loader == 'less') {
        loaders.push(
          {
            loader: 'less-loader',
            options: Object.assign({}, loaderOptions, {
              sourceMap: options.sourceMap
            })
          },
          {
            loader: 'sass-resources-loader',
            options: {
              // common.less 自己的公共变量路径
              resources: [path.resolve(__dirname, '../src/style/variables.less')]
            }
          }
        );
      } else {
        loaders.push({
          loader: loader + '-loader',
          options: Object.assign({}, loaderOptions, {
            sourceMap: options.sourceMap
          })
        })
      }
}

...

   // 新增一个全局使用less的函数
  function lessResourceLoader() {
    var loaders = [
      cssLoader,
      'less-loader',
      {
        loader: 'sass-resources-loader',
        options: {
          resources: [
            path.resolve(__dirname, '../src/style/variables.less') //定义全局变量的文件路径
          ]
        }
      }
    ];
    if (options.extract) {
      return ExtractTextPlugin.extract({
        use: loaders,
        fallback: 'vue-style-loader'
      });
    } else {
      return ['vue-style-loader'].concat(loaders);
    }
  }
// 修改less的配置
return {
	...
	less: lessResourceLoader(),
	....
}
```
## 四、现在可以在`vue`文件中开始使用`less`了
```html
<style lang="less" scoped>
	@box-color: #3370ff;
	.box {
		color: @box-color
	}
</style>
```