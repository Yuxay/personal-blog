---
title: CropperJS中文文档（翻译自Cropper.js原英文文档）
date: 2021-11-05 14:30:25
tags: js
categories: js
cover: https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=280591509,585529894&fm=26&gp=0.jpg
keywords: 'js,JavaScript,cropper.js'
aplayer: true
---

# Cropper.js

[![Downloads](https://img.shields.io/npm/dm/cropperjs.svg)](https://www.npmjs.com/package/cropperjs) [![Version](https://img.shields.io/npm/v/cropperjs.svg)](https://www.npmjs.com/package/cropperjs) [![Gzip Size](https://img.shields.io/bundlephobia/minzip/cropperjs.svg)](https://unpkg.com/cropperjs/dist/cropper.common.js) [![Dependencies](https://img.shields.io/david/fengyuanchen/cropperjs.svg)](https://www.npmjs.com/package/cropperjs)

> JavaScript image cropper.

- [官网](https://fengyuanchen.github.io/cropperjs)
- [Photo Editor](https://fengyuanchen.github.io/photo-editor) - Cropper.js的高级示例。
- [jquery-cropper](https://github.com/fengyuanchen/jquery-cropper) - Cropper.js 的 jQuery 插件包装器。
A schematic diagram for data's properties

## 目录

- [快速上手](#快速上手)
- [配置](#配置)
- [方法](#方法)
- [事件](#事件)
- [无冲突](#无冲突)
- [支持的浏览器](#支持的浏览器)
- [相关项目](#相关项目)

## 快速上手

### npm安装

```shell
npm install cropperjs
```

CDN:

```html
<link  href="/path/to/cropper.css" rel="stylesheet">
<script src="/path/to/cropper.js"></script>
```

[cdnjs](https://github.com/cdnjs/cdnjs) 为 Cropper.js's 的CSS 和JavaScript提供CDN支持. 你可以点击这个[链接](https://cdnjs.com/libraries/cropperjs).

### 开始使用

#### 基础用法

```js
new Cropper(element[, options])
```

- **element**
  - Type: `HTMLImageElement` or `HTMLCanvasElement`
  - 用于裁剪的目标图像或画布元素。

- **options** (optional)
  - Type: `Object`
  - 裁剪的配置. 详情查看 [配置](#配置).

#### Example

```html
<!-- 用块元素（容器）包裹图像或画布元素 -->
<div>
  <img id="image" src="picture.jpg">
</div>
```

```css
/* 确保图像的大小完全适合容器 */
img {
  display: block;

  /* 这个规则很重要，请不要忽略这个 */
  max-width: 100%;
}
```

```js
// import 'cropperjs/dist/cropper.css';
import Cropper from 'cropperjs';

const image = document.getElementById('image');
const cropper = new Cropper(image, {
  aspectRatio: 16 / 9,
  crop(event) {
    console.log(event.detail.x);
    console.log(event.detail.y);
    console.log(event.detail.width);
    console.log(event.detail.height);
    console.log(event.detail.rotate);
    console.log(event.detail.scaleX);
    console.log(event.detail.scaleY);
  },
});
```

[⬆ back to top](#目录)

## 配置

你可以使用`new Cropper(image, options)`设置裁剪器配置
如果要更改全局默认配置，可以使用 `Cropper.setDefaults(options)`.

### viewMode

- Type: `Number`
- Default: `0`
- Options:
  - `0`: 没有限制
  - `1`: 限制裁剪框不超过图片容器的范围。
  - `2`: 限制最片容器尺寸以在裁剪容器中展示。 如果图片容器和裁剪容器的比例不同，则图片容器以cover模式填充（图片容器保持原有比例，最长边和裁剪容器大小一致，短边等比缩放，可能会有部分区域不可见)。
  - `3`: 限制图片容器尺寸以在裁剪器中展示。 如果图片容器和裁剪容器的比例不同，则图片容器以contain模式填充（图片容器保持原有比例，最短边和裁剪容器大小一直，长边等比缩放，可能会有留白）。

定义裁剪器的视图模式。如果设置 `viewMode` 为 `0`,则裁剪框可以延伸到图片容器之外, 如果值是 `1`, `2`, 或 `3` 会将裁剪框限制为图片容器的大小。`viewMode` 的值为 `2` 或 `3` 时会限制图片容器在裁剪容器范围内。 当图片容器比例和裁剪容器比例一致时，`2`和`3`没有区别。

### dragMode

- Type: `String`
- Default: `'crop'`
- Options:
  - `'crop'`: 创建一个新的裁剪框
  - `'move'`: 图片容器可移动
  - `'none'`: 什么也不做

定义裁剪器的拖动模式

### initialAspectRatio

- Type: `Number`
- Default: `NaN`

定义裁剪框的初始宽高比。默认和图片容器的宽高比相同。

> 只有在 `aspectRatio` 的值为 `NaN`时才可以设置。

### aspectRatio

- Type: `Number`
- Default: `NaN`

定义裁剪框的固定宽高比。默认是自由比例

### data

- Type: `Object`
- Default: `null`

之前存储的裁剪后的数据，将在初始化时传递给`setData`方法

> 只有在 `autoCrop` 的值为 `true`时可用

### preview

- Type: `Element`, `Array` (elements), `NodeList` or `String` (selector)
- Default: `''`
- 一个元素或元素数组或节点列表对象或[Document.querySelectorAll](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelectorAll)的有效选择器

添加额外的元素（容器）预览裁剪后的结果。

**注:**

- 最大宽度是预览容器的初始宽度。
- 最大高度是预览容器的初始高度。
- 如果设置了`aspectRatio`属性，请确保将预览容器设置为相同的宽高比。
- 如果无法正确显示预览效果， 将预览容器的样式设置为 `overflow: hidden` 。

### responsive

- Type: `Boolean`
- Default: `true`

在窗口大小变化后，重新渲染裁剪器

### restore

- Type: `Boolean`
- Default: `true`

在窗口大小变化后，恢复被裁剪的区域

### checkCrossOrigin

- Type: `Boolean`
- Default: `true`

检查当前图片是否为跨域图片

如果是，将会在复制的图片元素上添加一个`crossOrigin`属性，并且会在src属性中添加一个时间戳参数，避免重新加载原始图片时因为浏览器缓存而加载错误。

向图片元素中添加`crossOrigin`属性将会阻止像图片路径中添加时间戳的操作并会停止图片的重新加载。但是读取图片数据以进行方向检查的请求(XMLHttpRequest)需要一个时间戳来破坏缓存，以避免浏览器缓存错误。可以通过设置`checkOrientation`为`false`来取消此请求。

如果图片的`crossOrigin`属性值为`"use-credentials"`，那么在通过XMLHttpRequest读取图片数据时，`withCredentials`属性会被设置为`true`

### checkOrientation

- Type: `Boolean`
- Default: `true`

检查当前图片的Exif信息中的Orientation值（方向信息，记录图片拍摄的相机的旋转信息）。请注意，只有JPEG图片可能会包含Orientation。

确切地说，读取旋转或翻转图片的Orientation，然后使用`1`(默认值)覆盖Orientation的值以避免在iOS设备上出现的问题([1](https://github.com/fengyuanchen/cropper/issues/120), [2](https://github.com/fengyuanchen/cropper/issues/509))。

必须同时设置 `rotatable` 和 `scalable` 为 `true`。

**注：**不要一直相信这一点，因为有些JPG图形可能会有不正确(非标准)的Orientation值。

> 需要 [Typed Arrays](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypedArray) 支持 ([IE 10+](https://caniuse.com/typedarrays)).

### modal

- Type: `Boolean`
- Default: `true`

是否在图片和裁剪框之间显示黑色蒙版。

### guides

- Type: `Boolean`
- Default: `true`

是否显示裁剪框的虚线。

### center

- Type: `Boolean`
- Default: `true`

是否显示裁剪框中心的指示器。

### highlight

- Type: `Boolean`
- Default: `true`

是否显示裁剪框上面的白色蒙版(突出显示裁剪框）。

### background

- Type: `Boolean`
- Default: `true`

是否在容器内显示网格背景。

### autoCrop

- Type: `Boolean`
- Default: `true`

是否允许在初始化时自动裁剪图片。

### autoCropArea

- Type: `Number`
- Default: `0.8` (80% of the image)

定义裁剪区域占图片的大小(百分比)。取值为 0 - 1。

### movable

- Type: `Boolean`
- Default: `true`

是否可以移动图片。

### rotatable

- Type: `Boolean`
- Default: `true`

是否可以旋转图片。

### scalable

- Type: `Boolean
- Default: `true`

是否可以缩放图片（以图片中心点为原点进行缩放）。

### zoomable

- Type: `Boolean`
- Default: `true`

是否可以缩放图片（以图片左上角为原点进行缩放）。

### zoomOnTouch

- Type: `Boolean`
- Default: `true`

是否允许通过拖动来缩放图片。

### zoomOnWheel

- Type: `Boolean`
- Default: `true`

是否允许通过鼠标滚轮缩放图片。

### wheelZoomRatio

- Type: `Number`
- Default: `0.1`

设置通过鼠标滚轮缩放图片时的缩放比例。

### cropBoxMovable

- Type: `Boolean`
- Default: `true`

是否允许通过拖动来移动裁剪框。

### cropBoxResizable

- Type: `Boolean`
- Default: `true`

是否允许通过拖动来调整裁剪框的大小。

### toggleDragModeOnDblclick

- Type: `Boolean`
- Default: `true`

是否允许双击切换图片容器拖拽模式(`"crop"`和`"move"`)

> 需要浏览器支持 [`dblclick`](https://developer.mozilla.org/en-US/docs/Web/Events/dblclick) 事件。

### minContainerWidth

- Type: `Number`
- Default: `200`

设置裁剪容器的最小宽度。

### minContainerHeight

- Type: `Number`
- Default: `100`

设置裁剪容器的最小高度。

### minCanvasWidth

- Type: `Number`
- Default: `0`

设置图片容器的最小宽度。

### minCanvasHeight

- Type: `Number`
- Default: `0`

设置图片容器的最小高度。

### minCropBoxWidth

- Type: `Number`
- Default: `0`

设置裁剪框的最小宽度。

**注:** 这个尺寸是相对于页面的，而不是图片。

### minCropBoxHeight

- Type: `Number`
- Default: `0`

设置裁剪框的最小高度。

**注：** 这个尺寸是相对于页面的，而不是图片。

### ready

- Type: `Function`
- Default: `null`

 `ready` 事件的快捷写法。

### cropstart

- Type: `Function`
- Default: `null`

 `cropstart` 事件的快捷写法。

### cropmove

- Type: `Function`
- Default: `null`

 `cropmove` 事件的快捷写法。

### cropend

- Type: `Function`
- Default: `null`

`cropend `事件的快捷写法。

### crop

- Type: `Function`
- Default: `null`

 `crop` 事件的快捷写法。

### zoom

- Type: `Function`
- Default: `null`

 `zoom` 事件的快捷写法。

[⬆ back to top](#目录)

## 方法

由于加载图片时有一个**异步**的过程，因此除了`setAspectRatio`、`replace`和`destroy`这几个方法，**大部分方法应该在ready之后调用**。

> 如果一个方法不需要返回任何值，它将会返回一个cropper实例，因此可以使用链式写法来调用多个方法。

```js
new Cropper(image, {
  ready() {
    // this.cropper[method](argument1, , argument2, ..., argumentN);
    this.cropper.move(1, -1);

    // 允许链式写法
    this.cropper.move(1, -1).rotate(45).scale(1, -1);
  },
});
```

### crop()

手动显示裁剪框。

```js
new Cropper(image, {
  autoCrop: false,
  ready() {
    // Do something here
    // ...

    // And then
    this.cropper.crop();
  },
});
```

### reset()

将图片和裁剪框重置为初始状态。

### clear()

清除裁剪框。

### replace(url[, hasSameSize])

- **url**:
  - Type: `String`
  - A new image url.
- **hasSameSize** (optional):
  - Type: `Boolean`
  - Default: `false`
  - 如果新图片和旧图片尺寸相同，就不会重建裁剪器，只会更新所有相关图片的url。这可以应用于过滤器。

替换图片路径并重建裁剪器。

### enable()

允许(解冻)裁剪。

### disable()

禁用(冻结)裁剪。

### destroy()

销毁裁剪器，并从图中移除实例。

### move(offsetX[, offsetY])

- **offsetX**:
  - Type: `Number`
  - 水平移动图片容器一定距离(px)。
- **offsetY** (可选):
  - Type: `Number`
  - 垂直移动图片容器一定距离(px)。
  - 如果不存在，默认和`offsetX`的值一致。

使用相对偏移量移动图片容器。

```js
cropper.move(1);
cropper.move(1, 0);
cropper.move(0, -1);
```

### moveTo(x[, y])

- **x**:
  
  - Type: `Number`
  - 图片容器在 `left`的值。
  
- **y** (可选):
  
  - Type: `Number`
  
  - 图片容器在`top`的值。
  
  - 如果不存在，默认和`x`的值一致。

移动图片容器到一个指定的点。

### zoom(ratio)

- **ratio**:
  - Type:  `Number`
  - ratio > 0：放大
  - ratio < 0：缩小

用相对比例缩放图片容器。

```js
cropper.zoom(0.1);
cropper.zoom(-0.1);
```

### zoomTo(ratio[, pivot])

- **ratio**:
  - Type: `Number`
  - 必须是一个大于0的数

- **pivot** (optional):
  - Type: `Object`
  - 格式: `{ x: Number, y: Number }`
  - 以裁剪容器的左上角为原点，设置缩放中心点的坐标

用绝对比例缩放图片容器。

```js
cropper.zoomTo(1); // 1:1 (canvasData.width === canvasData.naturalWidth)

const containerData = cropper.getContainerData();

// Zoom to 50% from the center of the container.
cropper.zoomTo(.5, {
  x: containerData.width / 2,
  y: containerData.height / 2,
});
```

### rotate(degree)

- **degree**:
  - Type: `Number`
  - degree > 0：度数大于0，向右旋转
  - degree < 0：度数小于0，向左旋转

用相对角度旋转图片。

> 需要 [CSS3 2D Transforms](https://developer.mozilla.org/en-US/docs/Web/CSS/transform) 支持 ([IE 9+](https://caniuse.com/transforms2d)).

```js
cropper.rotate(90);
cropper.rotate(-90);
```

### rotateTo(degree)

- **degree**:
  - Type: `Number`

旋转图片到一个绝对角度。

### scale(scaleX[, scaleY])

- **scaleX**:
  - Type: `Number`
  - Default: `1`
  - 设置图片缩放向量的横坐标。
  - 当值为`1`时，不进行缩放。
  - 当值为负数时，将图片进行横向镜面反转后再进行相应的缩放操作。
  
- **scaleY** (可选):
  - Type: `Number`
  - 设置图片缩放向量的纵坐标。
  - 当值为负数时，将图片进行纵向镜面反转后再进行相应的缩放操作。
  - 如果不存在，则值和 `scaleX`的值一致，从而使图片在保持原有形状下均等缩放。

通过向量形式定义的缩放值来放大或缩小图片，同时可以在不同的方向设置不同的缩放值。

> 需要 [CSS3 2D Transforms](https://developer.mozilla.org/en-US/docs/Web/CSS/transform) 支持([IE 9+](https://caniuse.com/transforms2d)).

```js
cropper.scale(-1); // 水平和垂直翻转
cropper.scale(-1, 1); // 水平翻转
cropper.scale(1, -1); // 垂直翻转
```

### scaleX(scaleX)

- **scaleX**:
  - Type: `Number`
  
  - Default: `1`
  
  - 设置图片缩放向量的横坐标。
  
  - 当值为`1`时，不进行缩放。
  
  - 当值为负数时，将图片进行横向镜面反转后再进行相应的缩放操作。

设置图片缩放向量的横坐标。

### scaleY(scaleY)

- **scaleY**:
  - Type: `Number`
  - Default: `1`
  - 设置图片缩放向量的纵坐标。
  - 当值为负数时，将图片进行纵向镜面反转后再进行相应的缩放操作。
  - 当值为`1`时，不进行缩放。

设置图片缩放向量的纵坐标。

### getData([rounded])

- **rounded** (optional):
  - Type: `Boolean`
  - Default: `false`
  - 将值设置为`true`时，会返回四舍五入后的值

- (return value):
  - Type: `Object`
  - Properties:
    - `x`: 被裁剪区域相对于图片左边的偏移量
    - `y`: 被裁剪区域相对于图片顶部的偏移量
    - `width`: 被裁剪区域的宽度
    - `height`: 被裁剪区域的高度
    - `rotate`: 原始图片的旋转度数
    - `scaleX`: 图片在横坐标上的缩放值
    - `scaleY`: 图片在纵坐标上的缩放值

返回最终裁剪的区域相对于原始图片的尺寸和位置数据。

> 你可以将数据发送到服务端来直接裁剪图片：
> 1. 使用`rotate`属性旋转图片。
> 2. 使用`scaleX`和`scaleY`属性来缩放图片。
> 3. 使用`x`,`y`,`width`和`height`属性来裁剪图片。

![数据属性的示意图](https://z3.ax1x.com/2021/11/12/IDKQl8.jpg)

### setData(data)

- **data**:
  - Type: `Object`
  - Properties: 查看[`getData`](#getdatarounded).
  - 在传入数据前，要先对数据进行四舍五入处理

设置裁剪区域相对于原始图片的尺寸和位置。

> **注:** 只有当`viewMode`的值大于等于`1`时，才可以使用这个方法.

### getContainerData()

- (return  value):
  - Type: `Object`
  - Properties:
    - `width`: 容器当前的宽度
    - `height`: 容器当前的高度

返回容器的尺寸数据。

> 示意图中各部分在本文档之中的称呼：
> container： 容器
> canvas： 图片容器
> crop box： 裁剪框
> image： 图片

![裁剪器的示意图](https://z3.ax1x.com/2021/11/12/IDuWLQ.jpg)

### getImageData()

- (return  value):
  - Type: `Object`
  - Properties:
    - `left`: 图片的左偏移量
    - `top`: 图片的上偏移量
    - `width`: 图片当前的宽度信息
    - `height`: 图片当前的高度信息
    - `naturalWidth`: 图片的初始宽度信息
    - `naturalHeight`: 图片的初始高度信息
    - `aspectRatio`: 图片的宽高比
    - `rotate`: 图片的旋转角度
    - `scaleX`: 图片在横坐标上的缩放值
    - `scaleY`: 图片在纵坐标上的缩放值

返回图片的位置、尺寸和其他相关数据。

### getCanvasData()

- (return  value):
  - Type: `Object`
  - Properties:
    - `left`: 图片容器的左偏移量
    - `top`: 图片容器的上偏移量
    - `width`: 图片容器的宽度
    - `height`: 图片容器的高度
    - `naturalWidth`: 图片容器的初始宽度信息（只读）
    - `naturalHeight`: 图片容器的初始高度信息（只读）

返回图片容器的位置和尺寸数据。

```js
const imageData = cropper.getImageData();
const canvasData = cropper.getCanvasData();

if (imageData.rotate % 180 === 0) {
  console.log(canvasData.naturalWidth === imageData.naturalWidth);
  // > true
}
```

### setCanvasData(data)

- **data**:
  - Type: `Object`
  - Properties:
    - `left`: 新的图片容器的左偏移量
    - `top`: 新的图片容器的上偏移量
    - `width`: 新的图片容器宽度
    - `height`: 新的图片容器高度

修改图片容器的位置和尺寸信息

### getCropBoxData()

- (return  value):
  - Type: `Object`
  - Properties:
    - `left`: 裁剪框的左偏移量
    - `top`: 裁剪框的上偏移量
    - `width`: 裁剪框的宽度
    - `height`: 裁剪框的高度

返回裁剪框的位置和尺寸信息

### setCropBoxData(data)

- **data**:
  - Type: `Object`
  - Properties:
    - `left`: 新的裁剪框的左偏移量
    - `top`: 新的裁剪框的上偏移量
    - `width`: 新的裁剪框的宽度
    - `height`: 新的裁剪框的高度

修改裁剪框的位置和尺寸信息

### getCroppedCanvas([options])

- **options** (optional):
  - Type: `Object`
  - Properties:
    - `width`: 输出的图片容器的宽度。
    - `height`: 输出的图片容器的高度。
    - `minWidth`: 输出的图片容器的最小宽度，默认值为`0`
    - `minHeight`: 输出的图片容器的最小高度，默认值为`0`
    - `maxWidth`: 输出的图片容器的最大宽度，默认值为`Infinity`
    - `maxHeight`: 输出的图片容器的最大高度，默认值为`Infinity`
    - `fillColor`: 填充输出的图片容器中任何 alpha 的值，默认值为`transparent`
    - [`imageSmoothingEnabled`](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/imageSmoothingEnabled): 如果图片是平滑的，设置此项以更改 (默认为`true`) 或不更改 (`false`).
    - [`imageSmoothingQuality`](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/imageSmoothingQuality): 设置图片平滑的质量, 值为 "low" (默认值), "medium", "high"中的一个.

- (return  value):
  - Type: `HTMLCanvasElement`
  - 一个绘制裁剪图片的图片容器

- 注:
  - 输出的图片容器的宽高比将自动与裁剪框的宽高比一致。
  - 如果你要从输出的图片容器中获取JPEG格式的图片，你应该首先设置`fillColor`属性，否则JPEG图片中的透明部分将默认变成黑色
  - 使用浏览器的原生API[canvas.toBlob](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCanvasElement/toBlob)来进行压缩，这意味着它是**有损压缩**。为了获得更好的图片质量，你可以将原始图片和裁剪数据上传到服务器，并在服务器上进行裁剪工作。

- 浏览器支持:
  - Basic image: 需要 [Canvas](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCanvasElement) 支持 ([IE 9+](https://caniuse.com/canvas)).
  - Rotated image: 需要 [CSS3 2D Transforms](https://developer.mozilla.org/en-US/docs/Web/CSS/transform) 支持 ([IE 9+](https://caniuse.com/transforms2d)).
  - Cross-origin image: 需要 HTML5 [CORS settings attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/CORS_settings_attributes) 支持 ([IE 11+](https://caniuse.com/cors)).

从裁剪的图片中得到一个图片容器（有损压缩），如果没有裁剪，则返回整个图片的图片容器

> 之后，如果浏览器支持这些API，你可以直接将这个图片容器显示为图片, 或者使用 [HTMLCanvasElement.toDataURL](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCanvasElement/toDataURL) 来得到一个 Data URL, 或者使用 [HTMLCanvasElement.toBlob](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCanvasElement/toBlob) 来得到一个 blob 并将其以 [FormData](https://developer.mozilla.org/en-US/docs/Web/API/FormData)上传到服务器

避免得到的图片是空白或者黑色的, 因为 [canvas元素的大小限制](https://stackoverflow.com/questions/6081483/maximum-size-of-a-canvas-element)，你需要将 `maxWidth` 和 `maxHeight` 属性设置为有限的数字. 此外,出于同样的原因，你应该限制最大缩放比例（在`zoom`事件中）

```js
cropper.getCroppedCanvas();

cropper.getCroppedCanvas({
  width: 160,
  height: 90,
  minWidth: 256,
  minHeight: 256,
  maxWidth: 4096,
  maxHeight: 4096,
  fillColor: '#fff',
  imageSmoothingEnabled: false,
  imageSmoothingQuality: 'high',
});

// 如果浏览器支持 `HTMLCanvasElement.toBlob`，将裁剪后的图片上传到服务器.
// `toBlob`的第二个参数默认值为'image/png',如果需要的话，可以修改它。
cropper.getCroppedCanvas().toBlob((blob) => {
  const formData = new FormData();

  // 如果需要的话，将图片的文件名作为第三个参数传递
  formData.append('croppedImage', blob/*, 'example.png' */);

  // 用`jQuery.ajax` 方法举例
  $.ajax('/path/to/upload', {
    method: 'POST',
    data: formData,
    processData: false,
    contentType: false,
    success() {
      console.log('Upload success');
    },
    error() {
      console.log('Upload error');
    },
  });
}/*, 'image/png' */);
```

### setAspectRatio(aspectRatio)

- **aspectRatio**:
  - Type: `Number`
  - 需要一个正数

修改裁剪框的宽高比。

### setDragMode([mode])

- **mode** (optional):
  - Type: `String`
  - Default: `'none'`
  - Options: `'none'`, `'crop'`, `'move'`

修改拖拽模式。

**提示:** 你可以通过双击裁剪器来切换`crop`和`move`模式

[⬆ back to top](#目录)

## 事件

### ready

当目标图片已加载完成，且裁剪器实例准备好操作时，触发此事件

```js
let cropper;

image.addEventListener('ready', function () {
  console.log(this.cropper === cropper);
  // > true
});

cropper = new Cropper(image);
```

### cropstart

- **event.detail.originalEvent**:
  - Type: `Event`
  - Options: `pointerdown`, `touchstart`, and `mousedown`

- **event.detail.action**:
  - Type: `String`
  - Options:
    - `'crop'`: 创建一个新的裁剪框触发
    - `'move'`: 移动图片容器触发
    - `'zoom'`: 通过触摸放大或缩小图片容器触发
    - `'e'`: 裁剪框的右边框触发
    - `'w'`: 裁剪框的左边框触发
    - `'s'`: 裁剪框的下边框触发
    - `'n'`: 裁剪框的上边框触发
    - `'se'`: 裁剪框的右下角触发
    - `'sw'`: 裁剪框的左下角触发
    - `'ne'`: 裁剪框的右上角触发
    - `'nw'`: 裁剪框的左上角触发
    - `'all'`: 移动裁剪框整体

当图片容器或裁剪框开始发生改变的时候触发

```js
image.addEventListener('cropstart', (event) => {
  console.log(event.detail.originalEvent);
  console.log(event.detail.action);
});
```

### cropmove

- **event.detail.originalEvent**:
  - Type: `Event`
  - Options: `pointermove`, `touchmove`, and `mousemove`.

- **event.detail.action**: 与"cropstart"一致.

当图片容器或裁剪框正在进行改变的时候触发

### cropend

- **event.detail.originalEvent**:
  - Type: `Event`
  - Options: `pointerup`, `pointercancel`, `touchend`, `touchcancel`, and `mouseup`.

- **event.detail.action**: 与"cropstart"一致.

当图片容器或裁剪框结束改变的时候触发。

### crop

- **event.detail.x**
- **event.detail.y**
- **event.detail.width**
- **event.detail.height**
- **event.detail.rotate**
- **event.detail.scaleX**
- **event.detail.scaleY**

> 关于这些属性，请查看 [`getData`](#getdatarounded) 方法.

当图片容器或裁剪框更改时触发

**Notes:**

- 当 `autoCrop`属性 被设置为 `true`时,  `crop` 事件将在 `ready` 事件之前被触发.
- 当设置了 `data` 属性时, 另一个 `crop` 事件将在 `ready` 事件之前被触发.

### zoom

- **event.detail.originalEvent**:
  - Type: `Event`
  - Options: `wheel`, `pointermove`, `touchmove`, and `mousemove`.

- **event.detail.oldRatio**:
  - Type: `Number`
  - 图片容器之前的比例

- **event.detail.ratio**:
  - Type: `Number`
  - 图片容器现在(之后)的比例 (`canvasData.width / canvasData.naturalWidth`)

当此案件器实例开始放大或缩小图片容器时，触发此事件

```js
image.addEventListener('zoom', (event) => {
  // Zoom in
  if (event.detail.ratio > event.detail.oldRatio) {
    event.preventDefault(); // Prevent zoom in
  }

  // Zoom out
  // ...
});
```

[⬆ back to top](#目录)

## 无冲突

如果必须使用具有相同命名空间的另一个裁剪器，只需要调用`Cropper.noConflict`静态方法即可恢复

```html
<script src="other-cropper.js"></script>
<script src="cropper.js"></script>
<script>
  Cropper.noConflict();
  // 使用其他`Cropper`的代码可以写在这里
</script>
```

## 支持的浏览器

- Chrome (latest)
- Firefox (latest)
- Safari (latest)
- Opera (latest)
- Edge (latest)
- Internet Explorer 9+

## 相关项目

- [angular-cropperjs](https://github.com/matheusdavidson/angular-cropperjs) by @matheusdavidson
- [ember-cropperjs](https://github.com/danielthall/ember-cropperjs) by @danielthall
- [iron-cropper](https://github.com/safetychanger/iron-cropper) by @safetychanger
- [react-cropper](https://github.com/react-cropper/react-cropper) by @roadmanfong
- [vue-cropperjs](https://github.com/Agontuk/vue-cropperjs) by @Agontuk

[⬆ back to top](#目录)