---
title: JS调起APP，没有app则跳转至应用市场
date: 2020-04-21 11:08:35
tags: js
categories: js
cover: https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=1767203791,1848851172&fm=26&gp=0.jpg
keywords: 'js,Javascript'
aplayer: true
---
### HTML
```html
<div class="container">
	<div id="btn_download" class="card-btn card_item">
    	立即下载
 	</div>
</div>
<div class="mask" @click="hide">
   <ol>
      <li>点击右上角</li>
      <li>浏览器打开</li>
   </ol>
</div>
```
### JS方法
```javascript
// 点击事件
document.querySelector('#btn_download').onclick = function () {
        toApp();
}
// 调起APP
    function toApp() {
        var btn_download = document.getElementById("btn_download");
        var ua = window.navigator.userAgent.toLowerCase();
        var isweixin =
            navigator.userAgent.toLowerCase().match(/micromessenger/i) != null;
        var isqq = navigator.userAgent.toLowerCase().indexOf("qq/") > -1;
        var isweibo = ua.match(/weibo/i) == "weibo";
        if (isweixin || isweibo) {
            document.getElementsByClassName("mask")[0].style.display = "block";
            document.body.addEventListener('touchmove', bodyScroll, false);
            document.documentElement.style.overflow = 'hidden';
            document.body.style.overflow = 'hidden';
            // 如果是安卓微信浏览器
            if (/android/i.test(navigator.userAgent)) {
                alert("安卓微信浏览器");
            }
            // 如果是iOS微信浏览器
            else if (/ipad|iphone|mac/i.test(navigator.userAgent)) {
                alert("iOS微信浏览器 ");
            }
        } else {
            if (/android/i.test(navigator.userAgent)) {
                if (!isweixin && !isqq) {
                    btn_download.setAttribute("href", "ops://");
                    this.openandIndex();
                }
            } else if (/ipad|iphone|mac/i.test(navigator.userAgent)) {
                if (!isweixin && !isqq) {
                    window.location.href = "com.dakang.mentalhealth://";
                    openiosIndex();
                } else if (isqq) {
                    btn_download.setAttribute("href", "com.dakang.mentalhealth://");
                    openiosIndex();
                }
            }
        }
    }
    // 安卓下载链接
    function openandIndex() {
        var t = setTimeout(function () {
            window.location.href = "******" //安卓的APP下载地址
        }, 3000);
        setTimeout(function () {
            clearTimeout(t);
        }, 3500);
    }
    // ios下载链接
    function openiosIndex() {
        var t = setTimeout(function () {
            window.location.href = "******"; //ios的APP下载地址
        }, 1500);
        setTimeout(function () {
            clearTimeout(t);
        }, 1500);
    }
```
### CSS样式
```css
.mask {
            position: absolute;
            width: 100%;
            height: 100%;
            top: 0;
            left: 0;
            display: none;
            text-align: center;
            line-height: 100%;
            font-size: 1.2rem;
            color: #333;
            background-color: rgba(242, 242, 242, 0.7);
        }

        ol {
            position: absolute;
            top: 30%;
            left: 35%;
        }

        ol li {
            margin-bottom: 30px;
        }
```