---
title: Javascript高阶函数AOP功能
date: 2016-03-29 15:21:26
tags: Javascript
categories: Front-End
---

### AOP实现代码

``` javascript
Function.prototype.before = function(beforefn) {
    var _self = this; //保存原函数的引用
    return function() { //返回包含了原函数和新函数的“代理”函数
    beforefn.apply( this, arguments); //执行新函数，修正this
        return _self.apply( this, arguments); //执行原函数
    }
};
Function.prototype.after = function(afterfn) {
    var _self = this; //保存原函数的引用
    return function() { //返回包含了原函数和新函数的“代理”函数
        var ret = _self.apply( this, arguments ); //执行原函数
        afterfn.apply( this, arguments ); //执行新函数，修正this
        return ret;
    }
};
```

### 调用例子

``` javascript
var example = function() {
    console.log("example函数执行:",2);
};
example = example.before(function() {
    console.log("example函数执行之前:",1);
}).after(function() {
    console.log("example函数执行之后:",3);
});
example();
```

### 执行结果

``` bash
example函数执行之前: 1
example函数执行: 2
example函数执行之后: 3
```