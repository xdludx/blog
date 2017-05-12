---
title: ES5新增的Array方法
date: 2017-05-12 13:51:23
tags:
---

### forEach()

描述：遍历数组，为数组中的每一个元素调用指定的函数。

案例：

```javascript
var arr = [1, 2, 3, 4, 5];
var sum = 0;
arr.forEach(function(cur, i, a) {
    // cur为当前值，i为当前索引，a为数组arr
    sum += cur;
});
console.log(sum); // 15
```
<!--more-->

### map()

描述：调用方式和`forEach()`一样，区别是必须有返回值，而且肯定会返回一个数组。

案例：

```javascript
var arr = [1, 2, 3, 4, 5];
var newArr = arr.map(function(cur, i, a) {
    // 这是数组中每个元素的返回值。
    return a[i] * cur;
});
console.log(newArr);
```

### filter()

描述：对数组进行过滤，返回过滤后的数组。

案例：

```javascript
var arr = [1, 2, 3, 4, , 5, 6];

var newArr = arr.filter(function(cur, i, a){
    // 这里会过滤掉原数组中缺少的元素。
    // return值如果为真，则添加到过滤数组。
    return i % 2 === 0;
});
console.log(newArr); // [1, 3, 6]
```

### every()和some()

描述：对数组元素进行逻辑判定，返回值为判定结果`true`或者`false`；

案例：

`every()`方法：

```javascript
var arr = [1, 2, 3, 4, 5];

console.log(arr.every(function(cur, i, a) {
    // 如果元素的每一个值都大于3，则返回true
    return cur > 3;
})); // false
```
`some()`方法：
```javascript
var arr = [1, 2, 3, 4, 5];

console.log(arr.some(function(cur, i, a) {
    // 如果有元素的值大于3，则返回true
    return cur > 3;
})); // ture
```

### reduce()和reduceRight()

描述：对数组元素化简求值。第一个参数是化简函数，第二个参数是一个可选参数，代表初始值。reduce()和reduceRight()的唯一区别就是求值的顺序。reduce()是从左向右求值，reduceRight()是从右向左求值。

案例：

```javascript
var arr = [1, 2, 3, 4, 5];

var rl = arr.reduce(function(pre, next) {
    return pre - next;
}); // -13。过程为 1-2-3-4-5=-13

var rr = arr.reduceRight(function(pre, next) {
    return pre - next;
}); // -5。过程为 5-4-3-2-1=-5
```

### indexOf()和lastIndexOf()

描述：按顺序搜索数组中有没有给定的元素值，如果有，则返回该元素的index（最早搜索到的），如果没有则返回-1。indexOf()和lastIndexOf()的唯一区别就是搜索的顺序不同。

案例：

```javascript
var arr = [1, 2, 3, 3, 2];
console.log(arr.indexOf(2)); // 1
console.log(arr.lastIndexOf(2)); // 4
console.log(arr.indexOf(4)); // -1
```
