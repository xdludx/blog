---
title: jQuery的ajax简易封装
date: 2017-05-26 17:34:05
tags: javascript
---

在项目中，经常会遇到需要点击一个按钮，向服务器请求资源的需求。这种情况如果我们不对ajax请求作任何处理，多次点击会有很多请求在等待，影响性能和体验。

<!--more-->

工作中，发现之前的项目代码是这样做的，感觉挺不错的，特拿出来分享一下。

```javascript
(function () {
    var ajax = {
        loading: {}, //确保每次的ajax请求必需在前一次ajax请求完成时执行
        //ajax获取数据
        ajax: function (_config) {
            _config = _config || {};
            //如果配置的url为"mock"，则success方法的参数是mock。
            if (_config.url === "mock" ) {
                _config.success ( _config.mock );
                return;
            }
            //如果ajax请求没有回来，则新建一个Deffered对象并返回，因为jquery的ajax方法也是返回一个Deferred。
            if ( ajax.loading[_config.url] ){
                return $.Deferred();
            }
            ajax.loading[_config.url] = true;

            var config = {
                type: 'post',
                dataType: 'json',
                contentType: 'application/json;charset=UTF-8',
                cache: false,
                complete: function (r) {
                    //ajax请求成功以后删除_config.url属性
                    delete ajax.loading[_config.url];
                }
            };
            $.extend( config, _config ); //覆盖掉默认属性
            return $.ajax(config);
        }
    };
    //将ajax方法放到util上
    (this.util = this.util || {}).ajax = ajax.ajax;

}).call(this);
```
