---
title: '一个有意思的题目foo.x = foo = {n: 2}'
date: 2017-08-17 18:43:02
tags:
---

今天逛技术博客的时候，遇到一个有意思的面试题：
```javascript
var foo = {n: 1};
var bar = foo;
foo.x = foo = {n: 2};
console.log(foo.x)
```

刚看到这个时候没觉着有水分，就是js中赋值符号 ‘=’的右结合性嘛，于是秒想结果是{n: 2}

而往下滚动屏幕，看到的答案确实
> undefined
> 
what？

看来这道题不是表象那么简单了。在浏览器控制台里测试了下

```javascript
foo.x; //undefined
foo; //{n: 2}
bar; //{n: 1, x: {n: 2}}
```

哦擦嘞！这题目还包含着对象引用关系的js知识。
然后就搜了搜这道题，还真有人单独问这道题的，这里以为大神[回答](https://segmentfault.com/a/1190000004224719)的挺专业.

不过他是以讲解知识为目的，顺带提起这个问题的，所以如果急于了解这个问题的前端们就可能短时间看不明白了。

所以这里仅针对这个题目来一个清晰点的解答。

--- 
<br><br><br>

##### js中的-[内存](http://www.jianshu.com/p/996671d4dcc4)
（已经会的可以跳过这段，愿意详细了解的，可以点击‘内存’链接）
> 在内存中js中5种基础类型的值（string number boolean null undefined）是直接保存在变量的值中的。

例如var a = 'abc';
在内存中表示为：![](https://ws3.sinaimg.cn/large/006tKfTcgy1fimvgckibjj30am03wdfu.jpg)
变量的值就是这个基础类型的数据；

<br><br>

> 其他类型的值如（object array function date regExp...等）是保存在内存堆中，只是暴露出来了一个引用地址
> 把这种类型的值赋值给变量，变量对应的值其实是这个值的引用地址

例如var b = {n: 1};
在内存中表示为：![地址的值是乱写的](https://ws1.sinaimg.cn/large/006tKfTcgy1fimvmw5op2j30jm0dajsd.jpg)

<br><br>
##### js中计算赋值是从右向左[计算](http://www.ximalaya.com/30018073/sound/13144303/)
（已经会的可以跳过这段，愿意详细了解的，可以点击‘计算’链接）

```javascript
var a = b = 6;
//js对上面代码赋值是这样的步骤
var a = undefined //预处理阶段：js的声明提前，先声明变量，并赋默认值undefined
b = 6; //执行阶段：把6赋值给b，同时这个赋值会返回这个值 6
a = 6; //再把返回值6赋值给a
```

--- 
<br><br>
##### 题目解析
简单理解上面两点，就可以结合来看这道题

```javascript
var foo = {n: 1}; 
var bar = foo;
foo.x = foo = {n: 2};
console.log(foo.x)
```
① 首先将{n: 1}的引用地址赋值给foo，内存中表现为：![](https://ws3.sinaimg.cn/large/006tKfTcly1fimvvolfg3j30ia0da0t4.jpg)

② 然后再将foo中的地址赋值给bar，内存中表现为：![](https://ws4.sinaimg.cn/large/006tKfTcgy1fimvymgcv2j30ku0dg3z9.jpg) 一个对象同时被两个变量引用着

③ foo.x = foo = {n: 2}

先将一个新的对象{n: 2}的引用地址赋值给foo，此时内存中表示为：![](https://ws1.sinaimg.cn/large/006tNc79gy1fimw5svhwlj30oo0eyq3u.jpg)关键就在种这里，给foo重新赋值完的一刻，foo已经跟{n: 1}没任何关系了。同时返回{n: 2}这个对象的引用地址

这一刻上面这段代码在js引擎里（js引擎预处理后，供计算机运行的代码，并非我们书写时的代码）相当于：
bar.x = foo = {n: 2}

因为在未执行这段代码的时候foo.x引用的是{n: 1}这个对象，而foo的值被改变后，原来的foo.x就相当于这一刻的bar.x了

然后再将返回的引用地址赋值给原来的那个对象{n: 1, x: undefined}里的x属性

此时内存中表现为：![](https://ws3.sinaimg.cn/large/006tNc79gy1fimwph9rwnj30pe0fimyb.jpg)

⑤ 所以打印foo.x的时候，js引擎就去找{n: 2}里面的x属性，没找到，就拿undefined回来交差了，

而此时bar变量引用的对象却有x属性