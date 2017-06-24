---
title: JSHint配置
date: 2017-06-24 23:49:22
tags: JSHint
---

### JSHint 简介

当我们做项目，发布代码时，大多会对需要上线的代码进行质量检查，我们公司用的是Jenkins+Sonar，如果Sonar检查不通过，代码则不能上线。基于这些考虑，我们有必要在编码阶段就保证自己的代码质量，JSHint就是这样一个工具。

### 相关配置

#### Enforcing options

当下面这些属性设置为`true`的时候，JSHint会对你的代码提供更多的警告。属性前边标记“！”的是已经废弃的属性，在JSHint的下个大版本就会移除，所以不建议使用。

##### bitwise

这个选项是禁止使用位运算符，比如^(XOR)，|(OR)或者其他运算符。位运算在JS中是极少用到的，我们很容易把`&`输成`&&`。

##### ! camelcase

这个选项是强制所有的变量必须用驼峰法或者下划线连接的全大写命名。如：`camelCase`或`UPPER_CASE`。

##### curly

这个选项要求我们必须将循环语句和条件语句放到花括号内。当语句块只有一句的时候，虽然JS允许我们省略花括号，但是在某些情况下，它可能会引发bug。

##### ! enforceall

这个选项是最严格的JSHint配置手段的快捷方式，在JSHint的2.6.3版本可以使用。它会启用所有的enforcing options，并且禁用所有的relaxing options。

##### eqeqeq

这个选项禁止使用`==`和`!=`，而是要使用`===`和`!==`。前者在进行比较之前会试图进行类型转换，因此会产生一些意想不到的结果。而后者就不会有这种问题。

##### ! es3

这个选项是告诉JSHint，你的代码需要遵循ECMAScript 3标准。如果你的程序需要兼容一些老的浏览器，比如IE 6/7/8，或其他比较古老的js环境，可以使用这个属性。

##### ! es5

这个选项要求语法首先要遵循ECMAScript 5.1规范。它包括了允许保留字作为对象的属性。

##### esversion

这个选项被用于指定代码需要遵循哪个ECMAScript版本，它可以指定一下几个值。

- 3 同`"es3": true`
- 5 同`"es5": true`
- 6 告诉JSHint，代码是基于ECMAScript 6的语法规范。注意不是所有的浏览器都支持这些语法。

##### forin

这个选项要求所有的`for in`循环都需过滤对象的属性。`for in`语句允许遍历一个对象所有的属性名称，包括继承自原型链上的属性。这种做法经常会导致你的对象上会包含一些以外的属性，所以安全起见，需要将原型链上的属性过滤掉，例如：
```javascipt
for (key in obj) {
    if (obj.hasOwnProperty(key)) {
        // We are sure that obj[key] belongs to the object and was not inherited.
    }
}
```

##### freeze

这个选项禁止重写原生对象的原型，比如：`Array`，`Date`等等。

##### futurehostile

这个选项允许警告代码中用到的JS在今后版本中的标识符，虽然在当前版本下，写这些标识符不会有什么问题，但是如果之后代码库要迁移到新的语言版本的时候，这种做法很可能会引发问题。

##### globals

这个选项用于指定一个在源码中没有正式定义的全局变量的白名单，当我们定义`"undef": true`的时候，这个选项就起作用了。用法如下:

```javascipt
    "globals": {
        "_": true,
        "util": true
    }
```

##### ! immed

这个选项要求函数的直接调用必须用小括号括起来。这样可以让其他人看你的代码时，更容易理解。

##### ! indent

这个选项用于指定你的代码缩进宽度。

##### latedef

这个选项是指变量在定义之前不能被使用。JS只具有函数作用域，除此之外，所有的变量都会被提升到函数的顶部。这种行为会导致一些令人讨厌的bugs，这也是只有在显式定义变量之后再使用它会更安全的原因。当设置属性`nofunc`的时候，将会允许忽略函数声明。

##### maxcomplexity

这个选项约束我们控制好代码的圈复杂度（Cyclomatic complexity），它通过程序的源代码测量线性独立路径数。

##### maxdepth

这个选项是设置有的代码块的嵌套深度。

##### maxerr

这个选项允许你设置警告的最大数量，如果超过这个数量就不在警告。默认50。

##### ! maxlen

这个选项是设置代码每行的最大长度。

##### maxparams

这个选项让你可以设置每个函数能允许参数的最大数量。

##### maxstatements

这个选项让你可以设置每个函数能允许的最大语句数。函数声明算作一条语句，它的函数体不被看做外部函数的语句。

##### ! newcap

这个选项要求你在构造函数命名必须首字母大写。首字母大写的函数可以和`new`操作符使用，这个习惯可以帮助程序员在使用`this`的情况下，清楚地判断出是构造函数还是其它类型的函数。

##### noarg

这个选项禁止使用`arguments.caller`和`arguments.callee`。`.caller`和`.callee`组织了大量的优化，所以将在JS将来的版本废弃。

##### nocomma

这个选项禁止使用逗号运算符。当使用不当时，逗号运算符会使语句值模糊，并导致错误的代码。

##### ! noempty

当你的代码中有空的语句块时，这个选项就会警告。

##### nonbsp

这个选项警告不换行空白字符。Mac的option + space是个非换行空白字符，对于非UTF8的页面会换行。

##### nonew

这个选项禁止了使用构造函数带来的一些副作用。有的人喜欢调用构造函数，而不将其赋值，比如：`new MyConstructor();`这样是没有好处的。

##### predef

这个选项允许你控制哪些变量JSHint可以视作环境中已经隐式定义了。用法：`"predef": [ "MY_GLOBAL", "ads" ]`

##### ! quotmark

这个选项强制在你的代码中使用引号的一致性。它接收三个值`[true|"single"|"double"]`。

##### shadow

这个选项检查变量的重复定义。比如在声明一个变量之前已经在外部的作用域声明过。允许有四个值。

- ["inner"|false] 仅仅检查同一作用域的变量声明
- ["outer"|true] 同时检查外部作用域的变量声明

##### singleGroups

如果不是必须使用分组操作符，这个选项会禁止使用。因为这是对一元运算符的误解。比如：
```
delete(boj.attr);
var a = 2 + (3 * 4);
```

##### strict

这个选项要求代码运行在ECMAScript 5的严格模式下。可以接收四个值：

- "global" 全局级使用严格模式
- "implied" 文件级使用严格模式
- false 禁止严格模式的警告
- true 函数级使用严格模式

##### trailingcomma

这个选项限制数组或对象的最后一个元素后不能加逗号。

##### undef

这个选项禁止使用未声明的变量。这个选项对于发现漏掉或写错变量的情况是非常有用的。

##### unused

当你定义了某个变量，却没有调用，这个选项会警告。这对于清理代码时是非常有用的。

##### varstmt

这个选项禁止使用`var`声明变量，而是用`let`或`const`代替。

#### Relaxing options

当下面这些属性设置为`true`的时候，JSHint会对你的代码提供更少的警告。属性前边标记“！”的是已经废弃的属性，在JSHint的下个大版本就会移除，所以不建议使用。

##### asi

这个选项对缺少分号的地方警告。

##### boss

这个选项警告应该在比较的地方进行了赋值。

##### debug

这个选项是允许代码中存在`debugger`。

##### elision

这个选项告诉JSHint，你的代码使用ES3规范中的数组省略元素或空元素。例如：`[1, , , 4, , , 7]`。

##### eqnull

这个选项会阻止`==null`发出的警告。这种比较经常会用于比较一个变量是`null`或者`undefined`。

##### ! esnext

指定使用ES6规范。等价于`"esversion": 6`。

##### evil

这个选项是取消对使用`eval`发出的警告。

##### expr

这个选项阻止应该使用复制或函数调用的地方使用了表达式而产生的警告。

##### funcscope

这个选项阻止了在语句块外部访问内部的变量而产生的警告。

##### ! globalstrict

等价于`"strict": "global"`。

##### iterator

这个选项阻止了关于__iterator__相关的报警。这个属性在所有的浏览器里边都不被支持，所以慎用。

##### lastsemic

这个选项阻止了缺少分号的警告。但是仅仅是一行语句块中的最后一条语句省略掉的分号。比如：`var name = (function() { return 'Anton' }());`。

##### ! laxbreak

这个选项阻止了不安全折行的警告。

##### laxcomma

这个选项阻止了逗号开始的警告。比如：

```javascipt
var obj = {
    name: 'Anton'
    , handle: 'valueof'
    , role: 'SW Engineer'
};
```

##### loopfunc

这个选项是阻止对循环内部的函数做出警告。

##### moz

这个选项是告诉JSHint，你的代码使用的是Mozilla的JS扩展，除非你是专门为火狐浏览器web开发，否则你不需要设置这个选项。

##### ! multistr

这个选项是阻止对多行字符串发出的警告。

##### notypeof

这个选项是阻止对无效的typeof操作符值发出的警告。默认情况下，JSHint会对`typeof a == 'fuction'`这种做出警告的。除非你确保不需要这样的检查，否则不要使用这个选项。

##### noyield

用了这个选项，在generator函数里边，没有`yield`语句，将不再警告。

##### plusplus

这个选项禁止使用一元运算符`++`和`--`。有的人认为`++`和`--`降低了他们代码风格的质量，而且有的编程语言，比如Python，完全没有这种操作符。

##### proto

这个选项禁止有关__proto__属性相关的属性。

##### scripturl

禁止使用scripts的url发出的警告，如 <a href="javascript:void 0"></a>

##### sub

禁止提示需要用`.`访问属性而不是`[]`，例如`person['name']`和`person.name`。

##### supernew

对`new function () { ... };`和`new Object;`这样结构的代码不再警告。

##### validthis

在严格模式下，对非构造方法内使用`this`不再警告。

##### withstmt

使用这个选项，不再对with语句发出警告。

#### Environments

这些属性让JSHint知道一些预先定义好的全局变量。

这些选项都比较容易理解，我就不一一解释了。这些选项包括：`browser`，`browserify`，`couch`，`devel`，`dojo`，`jasmine`，`jquery`，`mocha`，`module`，`mootools`，`node`，`nonstandard`，`phantom`，`prototypejs`，`qunit`，`rhino`，`shelljs`，`typed`，`worker`，`wsh`，`yui`。

### 后记

我平时使用的编辑器是atom，atom安装JSHint的方法很简单，直接去install列表，搜索`jshint`，然后install就可以了。写这篇文章的目的是了解一下都有哪些配置，遇到报错，能发现问题在哪，如果您看到这篇文章，并有幸帮助到您，那就更好不过了。

文章内容主要翻译自JSHint官网。链接为：[JSHint Options](http://jshint.com/docs/options/)
