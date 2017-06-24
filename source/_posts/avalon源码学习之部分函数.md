---
title: avalon源码学习之部分函数
date: 2017-06-19 15:11:41
tags: 
  - javascript
  - avalon
---

首先说一下，这里avalon的源码是指avalon 1.3.6版本，之所以看这个版本的源码仅仅是因为日常维护的系统中使用的就是这个版本，其实相较于其他的MVVM框架angular.js、vue.js，这个版本的avalon应该说已经很过时了。但是源码中有很多需要学习的点，对于我们还是很有借鉴意义的。

<!--more-->

这篇文章的主要目的是想简单介绍avalon源码中的一些方法。

### 比较两个值是否相等

```javascript
//比较两个值是否相等
var isEqual = Object.is || function(v1, v2) {
    if (v1 === 0 && v2 === 0) {
        // 这里是为了区分+0和-0
        return 1 / v1 === 1 / v2
    } else if (v1 !== v1) {
        // 这里是让NAN等于NAN
        return v2 !== v2
    } else {
        return v1 === v2
    }
}
```

这个方法也说明了ES6中`Object.is`这个方法的比较规则。

> 1. +0不等于-0
> 2. NAN等于自身

除去以上两个规则，`Object.is` 与 `===` 操作符的比较结果完全相同！

### 简化`console.log()`函数

```javascript
function log() {
    if (window.console && avalon.config.debug) {
        Function.apply.call(console.log, console, arguments)
    }
}
```

这段代码，不但简化了`console.log()`这个方法的调用，而且只是在debug模式下，才会打印日志信息。

源码中给出了引用地址[：How to safely wrap `console.log`?](https://stackoverflow.com/questions/8785624/how-to-safely-wrap-console-log)

### 判断变量类型

```javascript
var class2type = {};
"Boolean Number String Function Array Date RegExp Object Error".replace(/[^, ]+/g, function(name) {
    class2type["[object " + name + "]"] = name.toLowerCase();
})

var avalonType = function(obj) {
    if (obj == null) {
        return String(obj);
    }
    return typeof obj === "object" || typeof obj === "function" ?
        class2type[Object.prototype.toString.call(obj)] || "object" :
        typeof obj;
}
```

这段代码有几个技巧在里面。首先使用`/[^, ]+/g`这个正则表达式，可以将一个字符串通过空格或逗号隔开，然后再结合`replace()`方法，实现了字符串的`forEach`。然后由于对于数组，对象等一些变量，用typeof标识符，得到的结果都是`"object"`，在这里avalon用`Object.prototype.toString.call()`方法判断变量类型，并将得到的结果转化成我们想要的形式。