---
title: JavaScript 对象、构造函数、原型链之你想知道的一切
date: 2017-03-15 14:21:04
tags: [JavaScript, Object, __proto__, prototype, constructor]
---

本教程适合有一定基础的JavaScript爱好者阅读, 以下代码均在Chrome中运行。

## 对象

众所周知，JavaScript一切都是对象，普通函数是对象，Object、Array、String等内置对象都是对象，所谓对象就是：`{}`。
抛开Array、String等内置对象，JavaScript引擎提供了最基础的两种对象（我自己YY的），暂且分别称之为 __普通对象__、__函数对象__，其实它们的区别也仅仅是创建的时候属性不同而已。

<!-- more -->

### 普通对象
普通对象可以用`= {}`或者`new Object()`两种方式创建，从chrome中打印的结果可以发现两种方式生成的普通对象是相同的，下图是新建的__普通空对象__：
![img](./object.png) ![img](./newobject.png)
__普通空对象__只有一个\_\_proto\_\_属性。

### 函数对象
函数对象用`function`创建，函数对象的name属性就是函数名称，下图是新建的__空函数对象__：
![img](./function.png) ![img](./Objectis.png)
__空函数对象__有很多属性, Object实际上就是一个空函数对象，只不过现代浏览器为Object对象添加了很多私有方法。

## \_\_proto\_\_
每个对象都有一个\_\_proto\_\_属性，包括__普通对象__和__函数对象__，原型链上的对象正是依靠这个\_\_proto\_\_属性连结在一起的! (先忘记prototype这货吧)，查找属性时会沿着\_\_proto\_\_向上查找。
下面测试一下__普通对象__、__函数对象__、和内置的Object的\_\_proto\_\_属性。
```javascript
a = {}
function b(){}
b.__proto__ === Object.__proto__           //true 空函数对象的__proto__属性 和 内置Object的__proto__是相同的！小朋友们，思考下这意味着什么呢！！？
a.__proto__ === Object.__proto__.__proto__ //true 什么！！普通空对象的__proto__居然和空函数对象（Object）__proto__的__proto__居然是同一个对象！！
a.__proto__.__proto__ === null             //true 因此JavaScript中所有对象的顶层__proto__属性都是null
```

### Object.\_\_proto\_\_对象
Object.\_\_proto\_\_其实几乎和空函数对象没什么区别, 唯一的区别就是`b.name === "b"`而`Object.__proto__.name === ""`, 因为没有函数名字，因此function关键字是创造不出来这个对象的，只能JS引擎内置。
### Object.\_\_proto\_\_.\_\_proto\_\_对象
这也是一个JS引擎内置的对象，它的属性见普通空对象的\_\_proto\_\_属性，它的\_\_proto\_\_属性为null。

### prototype 和 new
不是所有的对象都有prototype属性，只有函数对象才有prototype属性（当然也可以给普通对象赋予prototype属性，但是没有什么意义）。
prototype和new关键字息息相关，prototype的作用只有在用new时才能体现出来，其他时候无用！！！（先不要反驳 \_\_proto\_\_ === prototype的情况）

```javascript
a = {} 或者 a = new Object() // a = {}和运用new关键字创建对象是相同的，这是一种特殊情况，相当于new
function b(){}
a.__proto__ === Object.prototype   //true
```
a对象只有一个\_\_proto\_\_属性，而它刚好恒等于Object.prototype，这绝不是巧合，这是new关键字起的作用，new过程中最关键的步骤就是将一个函数对象的prototype属性赋值给新的普通对象的__proto__属性，因此在a上查找属性时会向上查找a.\_\_proto\_\_属性，也就是Object.prototype属性。__因此prototype在原型链上其实没有什么作用__，假设将其它属性赋值给a.\_\_proto\_\_，它也会沿着这个属性向上查找的！！

## constructor

