---
title: JS判断变量类型的方法
date: 2018-03-05 15:08:52
tags: javascript
---

js判断变量类型的方法

<!--more-->

```javascript
let class2type = {};
let types = "Boolean Number String Function Array Date RegExp Object Error";
types.replace(/[^, ]+/g, (name) => {
    class2type["[object " + name + "]"] = name.toLowerCase();
})
let getType = (obj) => {
    if (obj == null) {
        return String(obj);
    }
    if (typeof obj === "object" || typeof obj === "function") {
        return class2type[Object.prototype.toString.call(obj)] || "object";
    } else {
        return typeof obj;
    }
}
```
以上方法来源于avalon源码，其中不包含一些es6变量的判断，如果需要加es6变脸类型判断需要在types字符串中添加相应变量。

在vue中的判断过程如下：

```javascript
let getType = (obj) => {
    return Object.prototype.toString.call(obj).slice(8, -1);
}
```