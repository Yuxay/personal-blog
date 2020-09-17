---
title: 使用electron-vue时遇到的问题及解决
date: 2020-05-26 10:18:44
tags: 
    - vue
    - electron
categories:
    - vue
cover: https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=1638427742,3961694984&fm=26&gp=0.jpg
keywords: 'vue,electron'
aplayer: true
---
# 1. 安装yarn及本地模块依赖
## 推荐使用管理员进行安装（win10右击开始栏windows选择Windows PowerShell(管理员)）
```javascript
npm install --global yarn
npm install -g node-gyp
npm install --global --production windows-build-tools
```
# 2. 创建electron-vue项目
```js
// 创建项目
vue init simulatedgreg/electron-vue my-project
// 安装依赖(有条件可通过科学上网进行安装，否则有些地方会卡住)
cd my-project
yarn // 或者 npm install
yarn run dev // 或者 npm run dev
```
# 3. 运行项目时遇到的问题
## 3.1. process is not defined
#### 在.electron-vue/webpack.renderer.config.js和.electron-vue/webpack.web.config.js文件中找到HtmlWebpackPlugin代码段并更改为如下代码：
```js
new HtmlWebpackPlugin({
      filename: 'index.html',
      template: path.resolve(__dirname, '../src/index.ejs'),
      templateParameters(compilation, assets, options) {
        return {
          compilation: compilation,
          webpack: compilation.getStats().toJson(),
          webpackConfig: compilation.options,
          htmlWebpackPlugin: {
            files: assets,
            options: options
          },
          process,
        };
      },
      minify: {
        collapseWhitespace: true,
        removeAttributeQuotes: true,
        removeComments: true
      },
      nodeModules: process.env.NODE_ENV !== 'production'
        ? path.resolve(__dirname, '../node_modules')
        : false
    }),
```
## 3.2. vue-devtools报错
[点此处下载devTools.zip并解压放在项目根目录中](https://pan.baidu.com/s/1-mEYAqa8M8tgRPx3WPDXsg) 提取码: aa84
#### 修改 src/main/index.dev.js
```js
// Install `vue-devtools`
require('electron').app.on('ready', () => {

  // 注释掉的这部分是 Electron-Vue 中预装devtool的代码，没有用
  // let installExtension = require('electron-devtools-installer')
  // installExtension.default(installExtension.VUEJS_DEVTOOLS)
  //   .then(() => {})
  //   .catch(err => {
  //     console.log('Unable to install `vue-devtools`: \n', err)
  //   })

  // 新增的：安装vue-devtools
  BrowserWindow.addDevToolsExtension(path.resolve(__dirname, '../../devTools/vue-devtools'));
  
});
```
# 4.打包项目时遇到的问题(使用 electron builder)
## 4.1. 打包时报错app-builder
### 4.1.1 下载打包所必需的文件
[nsis-resource-3.3.0.zip](https://pan.baidu.com/s/1a-c3R3HMMOi42cvDMR1RLQ) 提取码：7hb7
[nsis-3.0.3.2](https://pan.baidu.com/s/1GdOjyfBBpXgAUxtg-PQGRw) 提取码：b444
[winCodeSign-2.4.0](https://pan.baidu.com/s/1bz5DX4GyStj9Q5g5vL-JdA) 提取码:3ee9
[electron-v2.0.18-win32-x64.zip](https://pan.baidu.com/s/1ybeIKFdib0JNGxsQuA2-Hw) 提取码：nrq3
[chromedriver-v1.8.0-win32-x64.zip](https://pan.baidu.com/s/1TIh4GTxGH8A8k9nC9EbnPA) 提取码：gi8d
### 4.1.2. 打开 C:\Users\Administrator\AppData\Local\，按如下目录存放下载的文件
- electron
  * Cache
    + chromedriver-v1.8.0-win32-x64.zip
    + electron-v2.0.18-win32-x64.zip
 
 解压nsis-resource-3.3.0.zip，nsis-3.0.3.2，winCodeSign-2.4.0
- electron-builder
  * Cahe
    + nsis
    	- nsis-3.0.3.2(解压后的文件)
    	- nsis-resource-3.3.0(解压后的文件)
	+ winCodeSign
		- winCodeSign-2.4.0(解压后的文件)
## 4.2. 运行时日志中报错"Extension server error: Object not found: <top>", source: chrome-devtools://devtools/bundled/inspector.js
### 4.2.1 在main/index.dev.js文件中找到require('electron-debug')({ showDevTools: true });修改为
```js
 // NB: Don't open dev tools with this, it is causing the error
 require('electron-debug')();
```
### 4.2.2 在 main/index.js文件中找到 mainWindow.loadURL(winURL);在下方添加：
```js
 // Open dev tools initially when in development mode
  if (process.env.NODE_ENV === "development") {
    mainWindow.webContents.on("did-frame-finish-load", () => {
      mainWindow.webContents.once("devtools-opened", () => {
        mainWindow.focus();
      });
      mainWindow.webContents.openDevTools();
    });
  }
```
# 5.隐藏窗体菜单
## 在 src/main/index.js中修改
```js
	mainWindow = new BrowserWindow({
    	height: 563,
	    useContentSize: true,
    	width: 1000,
	    autoHideMenuBar: true,
  	})
  	mainWindow.setMenu(null)
  	mainWindow.loadURL(winURL)
```