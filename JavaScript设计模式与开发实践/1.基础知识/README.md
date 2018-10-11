# js设计模式与开发实践

## 基础知识

### 编程语言按数据类型分类：静态类型语言和动态类型语言
静态语言（以JAVA、C为例）在编译时便已确定变量的类型，动态语言（以JS为例）的变量类型要到程序运行的时候才会具有某种类型。
至于typescript是在编译的过程中做了一个静态类型检查，最后运行的还是js。

### 鸭子类型
概念：“如果它走起路来像鸭子，叫起来也像鸭子，那么它就是鸭子”
实例：一个对象如果有push和pop方法，并且这些方法能得到你想要的效果，那么它就可以当成栈来使用。
通过这个概念，我们可以做到“面向接口编程”。

### 面向对象的特征
多态：同一操作作用于不同的对象上面，可以产生不同的解释和不同的执行结果。

封装：将信息隐藏。包括封装数据、封装实现、封装类型和封装变化。

继承：

### 原型模式
基本规则：
```
1.所有的数据都是对象
2.要得到一个对象，不是通过实例化类，而是找到一个对象作为原型并克隆它。
3.对象会记住它的原型
4.如果对象无法响应某个请求，它会把这个委托给它自己的原型。
```
es6带来了新的class语法，但其背后的原理仍是通过原型机制来创建对象。

```
class Animal{
    constructor(name){
        this.name = name;
    }
    getName(){
        return this.name
    }
}
==>
function Animal(name){
    this.name = name;
}

Animal.prototype.getName = function () {
    return this.name;
}
----------------------------------------------------
class Dog extends Animal{
    constructor(name){
        super(name)
    }
    speak(){
        console.log("wook")
    }
}
==>
function Dog (name){
    Animal.call(this,name)
}
Dog.prototype = Object.create(Animal.prototype,{
    constructor:{
        value:Dog
    }
})
Dog.prototype.speak = function(){
    console.log("wook")
}

```
参考 <a href="https://www.jianshu.com/p/7144cd19c5e1"> ES5/ES6原型链与继承 </a>

### this、call和apply
this总是指向一个对象，而具体指向哪个对象是基于函数的执行环境动态绑定的，而非函数被声明时的环境。

call和apply的用途：
```
1.改变函数内部的this指向
2.Function.prototype.bind
3.借用其他对象的方法（常见场景：继承和使用array的push方法操作arguments）
```

### 闭包
当函数可以记住并访问所在的词法作用域时，就产生了闭包，即使函数是在当前此法作用域之外执行。（--你不知道的js（上））

### 高阶函数
指至少满足下列条件之一的函数：
1.函数可以作为参数被传递；2.函数可以作为返回值输出。

