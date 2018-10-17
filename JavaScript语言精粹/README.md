# JavaScript: The Good Parts - Douglas Crockford

## 1.精华

### JavaScript建立在一些非常优秀的想法和少数非常糟糕的想法上。

### 优秀的想法
* 函数

js中的函数（主要）是基于词法作用域的顶级对象，与其他语言使用的动态作用域不同。

当然在js中也存在动态作用域的使用，主要是体现在this的使用上。

* 弱类型

弱类型相比起强类型它并不会在编译的时候进行类型错误的检测，强类型这样做的目的是为了及早检测到错误并提醒开发者，但是在实际工作中，类型错误很多时候都不是导致程序错误的错误，而弱类型可以让开发者进行更加自由的开发。

* 动态对象

* 富有表现力的对象字面量表示法

通过列出对象的组成部分，它们就能简单地被创建出来，这也是JSON的灵感来源。

### 糟糕的想法
* 基于全局变量的编程模型

所有编译单元的顶级变量都被撮合到一个被称为全局对象的公共命名空间中。

----------

## 3.对象

在js中，除了简单数据类型（Number,String,Boolean,null,undefined）之外，其他所有的值都是对象。

## 对象字变量
对象字变量提供了一种非常方便地创建新对象值的表示法。一个对象字变量就是包围在一堆花括号中的零或多个“key:value”对。

-----------

## 4.函数
js中设计得最出色的就是它的函数的实现，但是也存在瑕疵。

因为在js中函数是一等对象，所以它也可以被当做参数在各个函数之间传递，可以很方便地使用各种设计模式。

## 有意思的特性
* 闭包

* 链式调用
* 函数柯里化
* apply调用
* ...

----------------
## 5.继承
js里面没有类的概念，所以它没有像那些基于类的语言中提供了 extends（ES6中的extends实际上是基于原型继承的，只不过提供了一个语法糖） 或 inheritance 这样的继承方法。

但是它可以模拟那些基于类的模式，也可以支持其他更具表现力的模式。

## 伪类 (构造函数)
```
var Sub = function(name) {
    this.name = name;
}

Sub.prototype = new Super();

Sub.prototype.methods = function(){}
```

与真正的类不同的是：伪类中没有私有环境，所有的属性都是公开的，无法访问super的方法。

这种形式可以给不熟悉js的程序员提供便利，但它也隐藏了该语言的真实的本质。在基于类的语言中，类继承是代码重用的唯一方式，但是js有更多而且更好的选择。

## 原型
概念：一个新对象可以继承一个旧对象的属性。
```
var super = {
    name: 'Super',
    getName: function() {
        return this.name;
    },
    doSomething: function () {
        return this.something;
    }
}

var sub = Object.create(super);
sub.name = 'Sub';
sub.something = 'say hi';
```
这种继承是一种<a href = "https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Differential_inheritance_in_JavaScript">差异化继承</a>

## 函数化
上面的方式都有一个弱点就是没法保护隐私，对象的所有属性都是可见的。不过还好我们有一个更好的选择就是应用模块模式。

首先我们要构造一个生成对象的函数。

1. 创建一个新对象。
2. 有选择地定义私有实例变量和方法。
3. 给这个新对象扩充方法。这些方法拥有特权去访问参数
4. 返回那个新对象。

```
var Super = function (name) {
    var that = {};
    that.get_name = function(){
        return name;
    }
    return that;
}
// var sub = Super("sub");
// or 
var sub = function(name) {
    var that = Super(name);
    that.say = function(str){
        return name + "say : " + str;
    }
    that.get_name = function(){
        return 'my name is ' + name;
    }
    return that;
}

// 如果我们想调用父类的方法同时在子类中重写这个方法要怎么做呢？
// 首先 我们定义一个方法
Object.prototype.superior = function(name) {
    var that = this, 
        method = that[name];
    return function () {
        return method.apply(that,arguments);
    }
}

var subsub = function(name) {
    var that = sub(name),
        super_get_name = that.superior('get_name');
    that.get_name = function() {
        return 'attention! ' + super_get_name();
    }
    return that;
}
```
---------
## 6.数组
在其他语言中，数组是一段线性分配的内存，它通过整体计算偏移并访问其中的元素。数组是一种性能出色的数据结构，但是js里面并没有数组这种数据结构。作为替代，js提供的是一种拥有一些类数组特性的对象。（从array.__proto__中可以看出array实际上是继承Function的）

-----------

## 糟糕的特性
* 全局变量： 全局变量在微型程序中可能会带来方便，但是随着程序越来越大，它们很快变得难以管理，因为一个全局变量可以被程序的任何部分在任意时间修改，使得程序的行为变得极度复杂。

* 保留字

* typeof ： typeof null = object;       typeof /a/ = object;

* 浮点数： 0.1+0.2 != 0.3

* 伪数组

----------------

## 有问题的特性
* ==

* with

* eval

* switch 穿越

