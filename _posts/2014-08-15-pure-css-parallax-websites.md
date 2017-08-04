---
layout: post
title:  "纯CSS视差滚动效果(初版)"
date:   2014-08-15 15:01:00 +0800
categories: css
---

原文：[Pure CSS parallax scrolling websites](http://blog.keithclark.co.uk/pure-css-parallax-websites/)

中文：[纯CSS视差滚动效果](http://blog.lanceli.com/2014/08/pure-css-parallax-websites.html ‎)

> 这篇文章主要讲述如何使用CSS transforms, perspective 和一些缩放技巧来制作纯CSS的视差滚动效果。

视差效果几乎都是用JavaScript实现的，往往使用了性能很差的__scroll__事件，在事件中立即修改DOM造成没必要的重绘。这些会造成浏览器渲染丢帧从而使得和滚动不同步。更优雅的视差实现应该是监听滚动并使用__requestAnimationFrame__延迟更新DOM - 但是如果我们完全不用JavaScript呢？

使用CSS的延迟视差效果避免了这些问题，并且浏览器硬件加速使得几乎所有负担被compositor直接处理。最终得多的是一直的帧速率和完全平滑的滚动。你当然也可以和其他的CSS特性一起使用，如[media queries](http://www.w3.org/TR/css3-mediaqueries/)或者[supports](http://www.w3.org/TR/css3-conditional/) - 怎么样？来个响应式的视差？

[DEMO](http://blog.keithclark.co.uk/wp-content/uploads/2014/08/demos/3/)

## 原理

开始之前让我们先准备下HTML：

```html
<div class="parallax">
  <div class="parallax__group">
    <div class="parallax__layer parallax__layer--back">
      ...
    </div>
    <div class="parallax__layer parallax__layer--base">
      ...
    </div>
  </div>
  <div class="parallax__group">
    ...
  </div>
</div> 
```

以及基本的样式：

```css
.parallax {
  perspective: 1px;
  height: 100vh;
  overflow-x: hidden;
  overflow-y: auto;
}
.parallax__layer {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
}
.parallax__layer--base {
  transform: translateZ(0);
}
.parallax__layer--back {
  transform: translateZ(-1px);
}
```

__parallax__ class是见证奇迹的奇迹的地方。一个元素定义了__高__和__透视__属性后将锁定视角中心，建立一个固定的3D源视图。设置__overflow-y: auto__允许容器内的元素按通常的方式滚动，但是后代元素将呈现相当于固定的角度。这是制作视差效果的关键点。

下一个是__parallax____layer__ class。如其命名，定义了视差效果应用于层内的内容。该元素是取出内容flow并填充容器。

最后是__parallax____layer--base__和__parallax____layer--back__，这些通过相对Z轴（移的越远，或是更接近于视区）变换确定视差元素的滚动速度。

[Try the demo](http://blog.keithclark.co.uk/wp-content/uploads/2014/08/demos/1/)

## 深入点

但是使用3D变形沿Z轴移动元素有个副作用 - 我们移动的或远或近会改变它实际的大小。得用__scale()__对元素进行放大到原来的大小。

```css
.parallax__layer--back {
  transform: translateZ(-1px) scale(2);
}
```

放大的计算公式为 __1 + (translateZ * -1) / perspective)__。例如，__perspective__ 设为 __1px__，Z轴移动**-2px**，那么矫正后的放大系数就是**scale(3)**：

```Css
.parallax__layer--deep {
  transform: translateZ(-2px) scale(3);
}
```

[Try the depth corrected demo](http://blog.keithclark.co.uk/wp-content/uploads/2014/08/demos/2/)

## 创建不同的视差页面区域

前面的例子使用很简单的内容示范了基本的技术， 但是多数视差页面都分有不同的区域，且有不同的效果。看怎么做。

首先，用**parallax__group**元素把所有层聚合在一起：

```html
<div class="parallax">
  <div class="parallax__group">
    <div class="parallax__layer parallax__layer--back">
      ...
    </div>
    <div class="parallax__layer parallax__layer--base">
      ...
    </div>
  </div>
  <div class="parallax__group">
    ...
  </div>
</div>
```

CSS：

```css
.parallax__group {
  position: relative;
  height: 100vh;
  transform-style: preserve-3d;
}
```

这个例子里，我想把每个组都填满视图，所以设置了**height:100vh**，不过如果需要的话你可以给每个组设置任意的值。**transform-style: preserve-3d**防止浏览器把**parallax__layer**元素变平，**position: relative**用来使**parallax__layer**元素能相对group元素定位。

有一点要牢记在心，我们不能修剪组内的内容。设置**overflow: hidden**将破坏视差效果。修剪的内容将导致子代元素超出，所以我们需要有创造性的带有z-index的组，确保当用户滚动时内容正确的显示或隐藏。

处理分层以实现不同的设计并没有严格的规定。如果你能看到视差如何工作那就很容易调试分层了，对group元素应用下面的样式吧：

```css
.parallax__group {
    transform: translate3d(700px, 0, -800px) rotateY(30deg);
}
```

试试下面的例子，注意**debug**选项

[Try the grouped parallax demo](http://blog.keithclark.co.uk/wp-content/uploads/2014/08/demos/3/)

## 浏览器兼容

- Firefox, Safari, Opera 和 Chrome都支持
- Firefox对齐时有点小问题
- IE还不支持**preserve-3d**(快了貌似)，所以还用不了。其实也没什么，你仍然要设计一个不支持视差效果的版本。你懂的，逐步提高。](https://gist.github.com/lanceli/3248619)