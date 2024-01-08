---
title: 作用域与变量提升
date: 2023-11-29 12:31:00
categories:
  - Front end
  - JavaScript
  - 作用域
tags:
  - JavaScript
description: 作用域与变量提升，函数提升
---

## 作用域

### 概念

在 javascript 语言中， 作用域是指一个变量或函数生效的范围，超出该范围则变量或函数无效（相当于未定义）。变量或函数定义时就会确定其作用域。其目的是为了减少命名冲突，提高程序的可用性与稳定性。

在 javascript 中有 3 种作用域：

- 全局作用域： 作用于整个浏览器标签生命周期，在页面打开时创建，页面关闭时销毁。
- 函数作用域： 作用于函数内部环境。
- eval 执行作用域： eval 方法调用时所处的作用域，若在函数中调用，则为函数作用域；若在顶层调用，则为全局作用域。

### 全局作用域与 window 对象

在页面生命周期内，代码的任何区域都可以访问到区域即全局作用域。

全局作用域在页面打开时创建，在页面关闭时销毁。直接写在 script 标签中的代码，都在全局作用域。全局作用域中有一个全局对象 window。

window 是 BOM（Borwser Object Modal）的核心对象，它代表浏览器的一个实例。在浏览器中 window 对象既是通过 Javascript 访问浏览器窗口的接口，又是 ECMAScript 规定的 Global 对象。这就意味着在网页中定义的任何一个对象、变量和函数，都以 window 作为其 Global 对象。

由于 window 对象扮演着 Global 对象角色，因此所有在全局作用域中声明的变量、函数都会变成 window 对象的属性和方法。

如下方代码示例：

```js
var age = 18;
function sayAge(){
  console.log(this.age);
}

console.log(window.age);  // 18
sayAge();                 // 18
window.sayAge();          // 18
```

我们在全局作用域定义了一个变量 age 和一个函数 sayAge()，它们被自动添加到了 window 对象中，作为 window 对象的属性和方法，因此可以通过 window.age 访问到变量 age， 可以通过 window.sayAge() 调用 sayAge() 方法。

由于 sayAge() 被定义在全局作用域中，因此 this.age 被映射到 window.age，因此输出结果如上。

虽然定义全局变量与直接在 window 对象上定义属性可以达到同样的使用效果，但两都还有一点细微差异： 全局变量无法通过 delete 操作符进行删除，而直接定义在 window 对象上可以。

```js
var a = 1;
console.log(a);           // 1
console.log(window.a);    // 1
delete window.a;
console.log(a)            // 1


window.b = 2;
console.log(b);         // 2
console.log(window.b);  // 2
delete window.b;
console.log(b)          // Uncaught ReferenceError: b is not defined


c = 3; // 等同于 window.c = 3;
console.log(c);         // 3
console.log(window.c);  // 3
delete window.c;
console.log(c)          // Uncaught ReferenceError: c is not defined
```

### 函数作用域

由于 javascript 中没有块级作用域的概念。这意味着在块语句中定义的变量，实际上是在包含函数中而非语句中创建的，如下面的例子：

```js
function outputNumbers(count){
  for(var i = 0; i < count; i++){
    console.log(i);
  }
  console.log(i);
}
```

该函数中定义了一个 for 循环，其中变量 i 的初始值被设置为0.在 c++、Java 等语言中，变量 i 只会在 for 循环语句块中有定义， 循环一旦结束，变量 i 就会被回收。而在 javascript 中，亦是是定义在 outputNumbers() 函数的活动对象中的，因此从它有定义开始，就可以在函数内部随时访问，即使重复声明同一个变量也不会改变它的值。

```js
function outputNumbers(count){
  for(var i = 0; i < count; i++){
    console.log(i);
  }

  var i;
  console.log(i);
```

javascript 中使用 var 关键字允许重复定义变量，且不会有任何错误提示，它只会对后续的声明视而不见（但是会执行后续声明中的变量初始化）。匿名函数可以用来模仿块级作用域避免这个问题。

```js
(function(){
  // 这里是块级作用域
})()
```

以上代码定义并立即调用了一个匿名函数（IIFE）。