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
}
console.log(a); // ReferenceError: Can't find variable: a
```

<Image src="https://cdn.jsdelivr.net/gh/CuiBenyong/resources@main/let-block-can-not-find.jpg" />
