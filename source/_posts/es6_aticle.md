---
title: 简单的讲一讲es6常用的特性
comments: true
date: 2017-07-25 0:28
tags:
---


最近一段时间基本上忙着自己的事情，忙着学习vuex、webpack、es6还有要弄一个vue+nodejs+mysql搞一个简单点开源项目，也在研究微信小程序。正经的写博客也是头一次，利用这个时候写一篇博客试试，讲讲我学过后的es6一些知识。


## 什么是ES6

  ECMAScript 6.0（以下简称 ES6）是 JavaScript 语言的下一代标准，已经在2015年6月正式发布了。它的目标，是使得 JavaScript 语言可以用来编写复杂的大型应用程序，成为企业级开发语言。因为当前版本的ES6是在2015年发布的，所以又称ECMAScript 2015。
  虽然目前并不是所有浏览器都能兼容ES6全部特性，但越来越多的程序员在实际项目当中已经开始使用ES6了。上个周在segmentfault社区里面看到推送的一周周刊里就说道es7已经在六月底就正式发布了，我看这是一年一个版本的节奏，只能说前端的技术更新太快。所以作为前端开发者的你还不关注新的技术那就out了，不会es6的基础用法都不太算是一名前端开发者了。

<!--more-->
### 什么是Babel

Babel是一个广泛使用的ES6转码器，可以将ES6代码转为ES5代码，从而在现有环境执行。大家可以选择自己习惯的工具来使用使用Babel，具体过程可直接在Babel官网查看

####let 和 const 命令
在es6之前我们都是使用var来声明一个变量，在es6的时候新增了let、const命令，虽然这两个变量与var类似，但是实际运用中他俩都有各自的特殊用途。
首先来看下面这个例子：
```
{
  let a = 10;
  var b = 1;
}

a // ReferenceError: a is not defined.
b // 1
```
使用var声明的变量b在代码块之外还能得到1，但是用let定义的变量b在代码块之外却是报错。这是因为let声明的变量只在它所在的代码块有效
let命令在for循环场景里面最适合不过了
```
var a = [];
for (let i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 6
```
上面代码使用var来声明那就不一样了
```
var a = [];
for (var i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 10
```
上面代码中，变量i是var声明的，在全局范围内都有效。所以每一次循环，新的i值都会覆盖旧值，导致最后输出的是最后一轮的i的值。而使用let则不会出现这个问题。

const也用来声明变量，但是声明的是常量。一旦声明，常量的值就不能改变。
```
const PI = 3.1415;
PI // 3.1415

PI = 3;
// TypeError: Assignment to constant variable.
```
当我们尝试去改变用const声明的常量时，浏览器就会报错。
const有一个很好的应用场景，就是当我们引用第三方库的时声明的变量，用const来声明可以避免未来不小心重命名而导致出现bug：
```
const monent = require('moment')
```
### 变量结构（基础用法）
ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。

以前，为变量赋值，只能直接指定值
```
let a = 1;
let b = 2;
let c = 3;
```
ES6 允许写成下面这样。
```
let [a, b, c] = [1, 2, 3];
```
上面代码表示，可以从数组中提取值，按照对应位置，对变量赋值。

本质上，这种写法属于“模式匹配”，只要等号两边的模式相同，左边的变量就会被赋予对应的值。下面是一些使用嵌套数组进行解构的例子。
```
let [foo, [[bar], baz]] = [1, [[2], 3]];
foo // 1
bar // 2
baz // 3

let [ , , third] = ["foo", "bar", "baz"];
third // "baz"

let [x, , y] = [1, 2, 3];
x // 1
y // 3

let [head, ...tail] = [1, 2, 3, 4];
head // 1
tail // [2, 3, 4]

let [x, y, ...z] = ['a'];
x // "a"
y // undefined
z // []
```
如果解构不成功，变量的值就等于undefined。
除了数组可以解构外还有对象解构，反正我觉得很方便（滑稽表情）
```
let { foo, bar } = { foo: "aaa", bar: "bbb" };
foo // "aaa"
bar // "bbb"
```
对象的解构与数组有一个重要的不同。数组的元素是按次序排列的，变量的取值由它的位置决定；而对象的属性没有次序，变量必须与属性同名，才能取到正确的值。
```
let { bar, foo } = { foo: "aaa", bar: "bbb" };
foo // "aaa"
bar // "bbb"

let { baz } = { foo: "aaa", bar: "bbb" };
baz // undefined
```
上面代码的第一个例子，等号左边的两个变量的次序，与等号右边两个同名属性的次序不一致，但是对取值完全没有影响。第二个例子的变量没有对应的同名属性，导致取不到值，最后等于undefined。
如果变量名与属性名不一致，必须写成下面这样。
```
var { foo: baz } = { foo: 'aaa', bar: 'bbb' };
baz // "aaa"

let obj = { first: 'hello', last: 'world' };
let { first: f, last: l } = obj;
f // 'hello'
l // 'world'
```
### 模板字符串（template string）
这个东西也是非常有用，当我们要插入大段的html内容到文档中时，传统的写法非常麻烦，经常出现的场景就是使用jq append()的时候回写一大推，一不小心还可能写错，所以之前我们通常会引用一些模板工具库，比如mustache等等。
传统的JavaScript语言，输出模板通常是这样写的。
```
$('#result').append(
  'There are <b>' + basket.count + '</b> ' +
  'items in your basket, ' +
  '<em>' + basket.onSale +
  '</em> are on sale!'
);
```
ES6引入了模板字符串
```
$('#result').append(`
  There are <b>${basket.count}</b> items
   in your basket, <em>${basket.onSale}</em>
  are on sale!
`);
```

### 箭头函数
ES6 允许使用“箭头”（=>）定义函数。可能你在一些网友写的代码中看到，如果你看到了说明就是es的写法,这es6最常用的特效了。
```
var f = v => v;
```
等同于下面这种：
```
var f = function(v) {
  return v;
};
```
如果箭头函数不需要参数或需要多个参数，就使用一个圆括号代表参数部分。
```
var f = () => 5;
// 等同于
var f = function () { return 5 };

var sum = (num1, num2) => num1 + num2;
// 等同于
var sum = function(num1, num2) {
  return num1 + num2;
};
```
####没有局部this的绑定
和一般的函数不同，箭头函数不会绑定this。 或则说箭头函数不会改变this本来的绑定。
我们用一个例子来说明：
```
function Counter() {
 this.num = 0;
 this.timer = setInterval(function add() {
 this.num++;
 console.log(this.num);
 }, 1000);
}
var b = new Counter();
// NaN
// NaN
// NaN
// ...
```
使用了关键字new构造，Count()函数中的this绑定到一个新的对象，并且赋值给a
你会发现，每隔一秒都会有一个NaN打印出来，而不是累加的数字
我们来尝试理解为什么出错：首先函数setInterval没有被某个声明的对象调用，也没有使用new关键字，再之没有使用bind, call和apply。setInterval只是一个普通的函数。实际上setInterval里面的this绑定到全局对象的。我们可以通过将this打印出来验证这一点：
```
function Counter() {
 this.num = 0;

this.timer = setInterval(function add() {
 console.log(this);
 }, 1000);
}

var b = new Counter();
//Window {speechSynthesis: SpeechSynthesis, caches: CacheStorage, localStorage: Storage, sessionStorage: Storage, webkitStorageInfo: DeprecatedStorageInfo…}
```
当我们用箭头函数就能解决这个问题
```
function Counter() {
 this.num = 0;
 this.timer = setInterval(() => {
 this.num++;
 console.log(this.num);
 }, 1000);
}

var b = new Counter();
// 1
// 2
// 3
// ...
```
摘一下阮老师的教程：
箭头函数有几个使用注意点。

（1）函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。

（2）不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。

（3）不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。

（4）不可以使用yield命令，因此箭头函数不能用作 Generator 函数。

this指向的固定化，并不是因为箭头函数内部有绑定this的机制，实际原因是箭头函数根本没有自己的this，导致内部的this就是外层代码块的this。
### set函数
ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。

Set 本身是一个构造函数，用来生成 Set 数据结构。
```
// 例一
const set = new Set([1, 2, 3, 4, 4]);
[...set]
// [1, 2, 3, 4]
//列二
const s = new Set();
[2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x));
for (let i of s) {
  console.log(i);
}
// 2 3 5 4
```
以前我们取出数组里面的重复值会用到遍历，然后用indexOf方法去做:


```
 var arr = ['a','c','b','d','a','b']
        var arr2 = [];
        for(var i = 0;i<arr.length;i++){
            if(arr2.indexOf(arr[i])<0){
                arr2.push(arr[i]);
            }
        }
        arr2.sort();
        console.log(arr2);//["a", "b", "c", "d"]
```
如果用Set()来去除数组里面的重复值那就更加简单
```
    var arr = ['a','c','b','d','a','b']
    var arr2 = [...new Set(arr)]
    console.log(arr2);//["a", "b", "c", "d"]
```
更多关于Set函数的属性和方法可以看阮老师的教程。
再一次感谢您花费时间阅读这份没水平的文章，尝试第一次写好一点的博客，最近学习的es6，我觉得很多新特性在日常开发当中我们都可以用上，比如我现在很喜欢的前端框架Vue，FaceBook的react 都在使用es6，所以推荐大家使用起来，加快我们的开发效率。  欢迎吐槽。。。


2016 年 07月 25日

