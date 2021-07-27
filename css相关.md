# Css 相关

### 1 背景图片全屏

    .background {
        width: 100%;
        height:100%;
        position: fixed;
        background: url(../asset/images/bg.png) no-repeat ;
        background-size: 100% 100%;
        background-position: 0 0
    }

    .background {
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background-size: cover;
        background-repeat: no-repeat;
        background-position: center center;
    }

---

### 2 画 0.5px 的线

#### （1） 0.5px 横线

    .line{
    	position:relative;
    }

    .line:after{
    	content: "";
    	position: absolute;
    	left: 0;
    	top: 0;
    	width: 100%;
    	height: 1px;
    	background-color: #e0e0e0;
    	transform:scaleY(.5);
    }

####（2） 0.5px 竖线
.line{
content: "";
position: absolute;
top: 0;
bottom: 0;
z-index: 1;
width: 1px;
background-color: #e8e8e8;
transform:scaleX(.5);
transform-origin: left center;
}

#### （3） 0.5px 边框

    .bd-all{
     	position:relative;
    }


    .bd-all:after{
    	content: "  ";
    	position: absolute;
    	left: 0;
    	top: 0;
    	z-index:-1;
    	width: 200%;
    	height:200%;
    	border:1px solid #e0e0e0;
    	-webkit-transform-origin: 0 0;
    	transform-origin: 0 0;
    	-webkit-transform: scale(.5, .5);
    	transform: scale(.5, .5);
     }

---

### 3 a 标签去除默认样式

    text-decoration: none;
    outline: none;

---

### 4 两个 inline-block 元素同行不对齐

    两个 inline-block 的元素都按照默认的垂直对齐方式为什么会产生不同的对齐效果？
    原因在于其中一个元素使用了 overflow: hidden 属性，这一属性改变了 inline-block 元素的基线位置，导致这一元素上移。
    因此，同时设置两个元素的垂直对齐方式为非基线对齐的值，或为另一个元素添加 overflow 属性都可以解决这一问题。

    或给元素加上vertical-ailgn属性 top bottom middle都可以

---

### 5 画一条长虚线

    通过linear-gradient来画虚线
    .line{
    	margin-top: 20px;
    	height: 1px;
    	width: 200px;
    	background: linear-gradient(to right, #000000, #000000 20%, transparent 20%, transparent 40%,#000000 40%, #000000 60%, transparent 60%, transparent 80%,#000000 80%, #000000 100%);
    }

---

### 6 文本超过一行省略号

#### （1） 但单行省略

> 兼容性较好 但只支持一行

```css
div {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
```

#### （2） 两行省略

> 移动端兼容性较好，移动端多为 webkit 内核

```css
div {
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2; // webkit 内核
  -webkit-box-orient: vertical;
}
```

#### （3）兼容性较好的 css3 方案

```scss
@minxin text-ellipsis-multiple-compatibility($line: 3, $line-height: 1.6em) {
  position: relative;
  line-height: $line-height;
  height: $line-height * $line;
  overflow: hidden;
}

.text-ellipsis-multiple-compatibility {
  position: relative;
  line-height: $line-height;
  height: $line-height * $line;
  overflow: hidden;
  &::after {
    content: "...";
    font-weight: bold;
    position: absolute;
    bottom: 0;
    right: 0;
    padding: 0 20px 1px 45px;
  }
}
```

#### js 实现

```javascript
function ellipsizeTextElement($element) {
  var nodeList = document.querySelectorAll($element);
  var elements = Array.prototype.slice.call(nodeList, 0);
  elements.forEach(function (element) {
    var wordArray = element.innerHTML.split(" ");
    while (element.scrollHeight > element.offsetHeight) {
      wordArray.pop();
      element.innerHTML = wordArray.join(" ") + "...";
    }
  });
}
```

[参考地址](https://juejin.cn/post/6844904202922098701)

---

### 7 [各种定位](./article/position.md)

- [flex 定位](./article/flex.md)

---

### 8 移动端 overhidden 问题

    1、body加position:fixed;width:100%;height:100%。

    2、给要滚动的元素添加一个父级，设定高度，overflow：auto；

    3、html,body{height:100%;overflow:hidden}

    建议使用第三种，可以把overflow:hidden作为一个单独的隐藏类，更方便控制。

---

### 9 pc 端字体最小单位兼容问题

- chrome 认为汉字小 于 12px 就会增加识别难度，尤其是我们页面常用的中文字体宋体和微软雅黑，这时候就需要取消浏览器的自动调整功能

      chrome下设置字体9px

      .relievestyle{
      	-webkit-text-size-adjust:none;
      	font-size:9px;
      }

- text-size-adjust 属性用来设定文字大小是否根据设备(浏览器)来自动调整显示大小，safari 3.0+，chrome 1.0+可以支持。属性值可 以为三种：
- percentage：字体显示的大小；
- auto：默认，字体大小会根据设备/浏览器来自动调整； none:字体大小不会自动调整；

##### 需注意，不合理的使用-webkit-text-size-adjust:none 会造成许多不好的影响。比如将其定义为全局属性的话，在 Chrome 中当用户放大缩小页面（PC 上按住 Ctrl 滚动鼠标滚轮可缩放网页）的时候，文字却维持定义的大小而不放缩，给用户带来的不太友好的体验。在使用 时，最好定义为局部有效，而不要一句 html{-webkit-text-size-adjust:none;}就好了。

##### 在 PC 桌面版 Chrome 27 上正式取消了-webkit-text-size-adjust 属性的支持，实际上是修正了原有的 bug。如果定义该属性在 Chrome 调试窗 口会显示 Unknown property name，所有字号最小为 12px。但移动端 chrome/safari 目前依然支持-webkit-text-size-adjust 属性。因为此 属性的滥用会使得 webkit 内核的浏览器失去调节能力，对于有视觉障碍的浏览者非常不友好（尤其是移动终端）

    	可以通过以下代码缩放实现
    	.chrome_adjust {
    		font-size: 9px;
    		-webkit-transform: scale(0.75);
    	}

1.  Chrome 下由于启用了缩放，所以字符间距出现问题，影响了美观，这时候如果追求完美，可能你还会想到 js 判断为 Chrome 后再用 CSS 属性 letter-spacing 去修复；

2.  Firefox 不认识-webkit，所以表现正常，9px；
3.  Opera 12.5+能够识别-webkit-前缀（Opera 12.15 版本，内核暂未更换为 webkit，但是已能够识别-webkit-前缀了，而且在检查元素 时还抹掉了前缀），但又能够显示 12px 以下的字号，结果变成了 9×0.75，影响了肉眼的识别，这时候，又得给 opera 添加-o-transform: scale(1);这个属性。

        .browser_adjust {
        	font-size: 9px;
        	-webkit-transform: scale(0.75); //居中缩放 -o-transform: scale(1); //针对能识别-webkit的opera browser设置
        }

转自 GitHub[原文地址](https://github.com/islittle/Web-Developer/blob/master/css-notes/compatible-with-less-than-12px-fontsize.md)

---

### 10 css border 画半圆

    左半圆
    .circle1 {
        width: 100px;
        height: 200px;
        border: 1px solid black;
        border-radius: 100% 0 0 100%/50%;
        border-right: none;
    }

    上半圆
    .circle2 {
        width: 200px;
        height: 100px;
        border: 1px solid black;
        border-radius: 50% 50% 0 0/100% 100% 0 0;
        border-bottom: none;
    }

    右半圆
    .circle3 {
        width: 100px;
        height: 200px;
        border: 1px solid black;
        border-radius: 0 100% 100% 0/50%;
        border-left: none;
    }

    下半圆
    .circle4 {
        width: 200px;
        height: 100px;
        border: 1px solid black;
        border-radius: 0 0 50% 50%/0 0 100% 100% ;
        border-top: none;
    }

---

### 11 first-child first-of-type 伪类区别

1.  first-child 表示在一组兄弟元素中的第一个元素 [`mdn地址`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:first-child)
2.  first-of-type 表示一组兄弟元素中其类型`(同一个标签元素)`的第一个元素。 [`mdn地址`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:first-of-type)

        	<article>
        	  <div

    。>第一个 div</div>
    <div>测试 <span>第一个 span</span>!</div>
    <div>This <em>nested `em` is first</em>, but this <em>nested `em` is last</em>!</div>
    <div>This <span>nested `span` gets styled</span>!</div>
    <b>This `b` qualifies!</b>
    <div>This is the final `div`.</div>
    </article>

---

### 12 translate translation transform animate 动画

1.  **transition**在一定时间之内，一组 css 属性变换到另一组属性的动画展示过程

        .move-active {
        	opacity: 1;
        	left: 0;
        }

        .div {
        	transition: left 1s ease-in ,opacity 2s ease-in;
        	opacity: 0;
        	left: 300px;
        }
        transition: all .5s ease-in;

        通过动态添加 active 类名来控制动画

2.  transform 是变换、变形，是 css3 的一个属性，和其他 width，height 属性一样
3.  translate 是 transform 的属性值，是指元素进行 2D 变换

        transform：translate(-20px,0)
