---
title: 认识关键字 var/let/const
date: 2023-11-29 15:31:00
categories:
  - Front end
  - JavaScript
  - 基本语法
  - 变量及定义
tags:
  - JavaScript
description: var/let/const 使用及区别
---

## 变量声明

在 ES5 中， 我们使用 `var` 来声明变量。

在 ES6 中， 新增了 `let` 和 `const` 关键字：

- let: 替代 `var`， 定义变量，声明后可修改其值。
- const: 定义常量，需要声明时赋值初始化，赋值后无法修改（尝试修改会编译出错）。

### 作用域介绍

请参考[作用域与变量提升](/9de4d0076bb6/)

### var 变量定义及使用

如下代码，控制台将输出 `2`。这是因为 var 声明的变量属于全局作用域，即使在区域中声明，也会被挂载到全局作用域中，即 `var` 声明的变量不具备块级作用域特性。

```js
{
  var a = 2;
}
console.log(a); // 1
```

<Image src="https://cdn.jsdelivr.net/gh/CuiBenyong/resources@main/var-block-global.jpg" />

如下代码，控制台将输出 `2`。这同样是因为 var 声明的变量属于全局作用域。

```js
var a = 1;
{
    var a = 2;
}

console.log(a); //这里的 a，指的是区域里被重新定义的 a
```

<Image src="https://cdn.jsdelivr.net/gh/CuiBenyong/resources@main/var-block-redefined-global.jpg" />

> 因此，在 ES5 语法中，使用 var 定义的变量，容易造成全局变量污染，比如：若出现变量定义重名的情况时，先被定义的变量会被后被定义的变量覆盖，导致后续变量使用时不符合我们的需求。 因此在开发中我们应该尽量避免使用 var 来定义变量。尽可能使用 ES6 中的 `let` 或 `const` 来定义。

### let 定义变量及使用

如下代码，在运行时会报错。 这是因为 let 声明的变量只在当前块级作用域中生效，超出该块级作用域使用会报未找到变量的异常。

```js
{
  let a = 1;
  var b = 2;
}
console.log(a); // ReferenceError: Can't find variable: a
console.log(b); // 2
```

<Image src="https://cdn.jsdelivr.net/gh/CuiBenyong/resources@main/let-block-can-not-find.jpg" />

在 for 循环计数器就适合使用 let 命令。

```js
for (let i = 0; i < 10; i++) {
  // ...
}
console.log(i); // ReferenceError: i is not defined
```

上述代码中，计数器 i 只在 for 循环内部有效，在循环外使用就会报错。

下面的代码如果使用var，最后输出的是10。

```js

for (var i = 0; i < 10; i++) {
}
console.log(i); // 10

```

上面代码中，变量i是var命令声明的，在全局范围内都有效，所以全局只有一个变量i。每一次循环，变量i的值都会发生改变，运行到最后一轮 i 的值变成 10。

另外 for 循环还有个特殊之处，就是设置循环变量的那部分是一个父作用域，而循环体内部是一个单独的子作用域。

```js
for (let i = 0; i < 3; i++) {
  let i = 'abc';
  console.log(i);
}
// abc
// abc
// abc
```

上面代码正确运行，输出了 3 次abc。这表明函数内部的变量i与循环变量i不在同一个作用域，有各自单独的作用域，此处的 i 不存在重复定义的情况。

### const 定义常量及使用

在开发中，如果希望变更声明后不再发生变化，则可以使用 `const` 来定义为常量，常量定义之后地址无法再改变，如果去修改了该常量，则会在编译时报错。例如：

```js
const hello = 'hello world'; // 定义常量 hello 值为 'hello world'

hello = 'hello xuanyi'; //  TypeError: Assignment to constant variable.

const world; // SyntaxError: Missing initializer in const declaration
```

> 注意： `const` 定义常量时必须立即赋值，否则报错。


### let 与 const 特点

- 局部作用域生效，不属于 window 对象
- 不能重复定义
- 不存在变量提升
- 存在暂时性死区（DTC）
- 支持块级作用域

而 var 定义的变量：存在变量提升、可以重复定义、不支持块级作用域（定义即全局）。

### var/let/const 共同点

- 外部定义的变量，可以在函数或块级作用域使用
- 函数中定义的变量，只能在函数及其子函数中使用，函数外部无法使用。

## 暂时性死区（DTC）

只要块级作用域内存在 `let` 关键字，它所声明的变量就“绑定”（`binding`）这个区域，不再受外部的影响。

```js
var tmp = 123;
if (true) {
  tmp = 'abc'; // ReferenceError
  let tmp;
}
```

上面代码中，存在全局变量 tmp，但是块级作用域内let又声明了一个局部变量 tmp，导致后者绑定这个块级作用域，所以在 let 声明变量前，对 tmp 赋值会报错。

ES6 明确规定，如果区块中存在 let 和 const 命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错。

总之，在代码块内，使用 let 声明变量之前，该变量都是不可用的。这在语法上，称为“暂时性死区”（temporal dead zone，简称 TDZ）。

