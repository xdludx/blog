---
title: JS事件对象
date: 2017-08-24 15:59:34
tags: javascript
---

事件对象在js中是很重要的一部分，用户和系统进行的任何交互都是通过事件来进行的。在除了IE8及之前版本的所有浏览器，对于所有的文档元素都定义了个addEventListener()的方法，用这个方法可以为事件目标注册事件处理程序。

<!--more-->

### 冒泡和捕获

```html
<div>
    <span>test</span>
</div>
```
在标准事件模型中，同时支持冒泡和捕获两个不同的事件流，对于以上文档结构，事件冒泡的顺序为`span` -> `div` -> `body` -> `html` -> `document`，而事件捕获的顺序则与冒泡完全相反，顺序为`document` -> `html` -> `body` -> `div` -> `p`。

下面这段代码就能很清晰的解释文件流的顺序。

```html
<div class="outer">
    <div class="inner">
        点击事件
    </div>
</div>
<script>
    var outer = document.querySelector(".outer");
    var inner = document.querySelector(".inner");
    outer.addEventListener("click", function() {
        console.log("outer 捕获");
    }, true);
    outer.addEventListener("click", function() {
        console.log("outer 冒泡");
    }, false);
    inner.addEventListener("click", function() {
        console.log("inner 捕获");
    }, true);
    inner.addEventListener("click", function() {
        console.log("inner 冒泡");
    }, false);
</script>
```
以上代码的输出顺序是`outer 捕获` -> `inner 捕获` -> `inner冒泡` -> `outer冒泡`。

### Target和currentTarget

事件的这两个属性经常被弄混，下面总结一下这两个属性的区别。

`Target`是用户的点击事件作用到的元素，也就是事件的目标，而`currentTarget`是事件冒泡或者捕获阶段，事件处理程序当前正在处理事件的那个元素。所以一般情况下，我们使用`Target`来确定我们实际点击的哪个元素，而使用`currentTarget`来阻止冒泡等行为。

在事件处理程序内部，对象`this`始终等于`currentTarget`的值。

### 事件代理

事件代理也叫事件委托，是针对“事件处理程序过多”问题的解决方案，它利用事件冒泡，指定一个事件处理程序，从而管理一类的所有事件。我们经常会见到点击一个列表，如下所示。
```html
<style>
    .item {
        background: #ffffff !important;
    }
</style>
<ul class="lists">
    <li class="item" data-color="#FF0000">赤</li>
    <li class="item" data-color="#FF7F00">橙</li>
    <li class="item" data-color="#FFFF00">黄</li>
    <li class="item" data-color="#00FF00">绿</li>
    <li class="item" data-color="#00FFFF">青</li>
    <li class="item" data-color="#0000FF">蓝</li>
    <li class="item" data-color="#8B00FF">紫</li>
</ul>
<script>
    var outer = document.querySelector(".lists");
    var item = document.querySelectorAll(".item");
    outer.addEventListener("click", function(e) {
        item.forEach(function(i) {
            i.className = "item";
        });
        e.target.className = "";
        e.currentTarget.style.backgroundColor = "#ffffff";
        e.target.style.backgroundColor = e.target.dataset.color;
    }, false);
</script>
```

先就这么多吧，遇到问题之后再补充~