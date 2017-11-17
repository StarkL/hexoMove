---
title: JS模块化编程
date: 2017-06-28 16:25:12
tags:
---

#### 为什么要用它
JS发展越来越好，ECMAJavaScript也逐年更新版本，同时浏览器再跟随更新，JS的原生能力越来越强。
早就想做一个自己的组件库，然而只是零零碎碎的把方法封装成一个function
```javascript
function doSomething() {
  //code
}
```
然后在调用的时候再通过方法名来调用。

不难发现，
1. 这样如果有很多方法，如果在一个项目里调用自己的组件，会污染全局变量，项目里对函数命名的时候还得避开自己组件的名字
2. 而且各个组件相互独立，难免会有某个组件要在另一个组件的基础上进行二次开发，这样组件之间的关系也不明确，组件库维护起来很费力。

所以不得不面临模块化的问题：将自己的组件库做成一个像是jquery一样的插件来用。

#### 模块化是什么
经过一段时间现场搜索，模块化简单的说，就是一个完整的一块砖，哪里需要就能往哪搬。 不需要理解它怎么做出来的，只需要知道它是怎么用的就行。就像jquery插件，可以当做是一个大模块。

#### 模块化的标准
先例里有 commendJS 和 AMD 两个规范，后者更适合浏览器端的开发。

简要的说，模块化需要做到：
1. 单体化：是一个模块，来实现一组(或一大堆)特定的功能
2. 安全化：模块内的内容不能被外部代码影响，如果jquery的方法里的内容暴露出来的，那么我们用jqueyr写代码的时候很可能会不小心自己的代码与jquery暴露的代码冲突，导致jquery某个功能不能用，或者自己的某段代码异常
3. 允许传入参数：很多组件上都需要根据传入参数来确定具体执行什么样的代码，就像obj.css({height:'50px'})一样，jquery接收了参数后，调用内部代码执行样式修改

当然，模块化还有一些细节需求，例如加载顺序等问题，这里先做基础的模块化。

#### 一个一个地达到模块化标准

一、单体化

要求自己的组件库是一个整体的库，里面有很多自己定义的功能化组件。如果原来的自己的几个组件是：
```javascript
function tool1(){ code };
function tool2(){ code };
function tool3(){ code };
```
那么这是三个方法，为做到单体化，把他们放在一个对象里或者方法里即可：
```javascript
var myComponents = function() {
  name: 'zhangsan';
  function tool1(){ code };
  function tool2(){ code };
  function tool3(){ code };
  return {
    name: name,
    tool1: tool1,
    tool2: tool2,
    tool3: tool3
  }
}
//调用方法
myComponents.tool1();
```

二、安全化
组件里的属性和值应该是不可以被外部修改的，上面的代码是可以通过
```javascript
myComponents.name = 'lisi';
```
来进行修改的。
要想保证组件库里面的代码不暴露出来，可以用闭包来解决。
```javascript
var myComponents = (function() {
  name: 'zhangsan';
  function tool1(){ code };
  function tool2(){ code };
  function tool3(){ code };
  return {
    name: name,
    tool1: tool1,
    tool2: tool2,
    tool3: tool3
  }
})();
```
闭包来解决这个问题再合适不过了。

而这里每次要return多个参数，可以将他们再放入到一个对象里，返回一个对象就可以了。
```javascript
var myComponents = (function() {
  var comp = {};
  comp.name: 'zhangsan';
  comp.tool1 = function (){ code };
  comp.tool2 = function (){ code };
  comp.tool3 = function (){ code };
  return comp;
})();
```

三、允许传入参数
上面用了'匿名函数立即执行'的方法形成了一个闭包，这里可以将需要传入的参数传入这个执行函数的小括号
```javascript
var myComponents = (function(window) {
  var comp = {};
  if(window.str) {
  		var _str = str;
  }
  comp.name = 'zhangsan';
  comp.tool1 = function (_str){ alert(_str) };
  comp.tool2 = function (){ alert('tool2') };
  comp.tool3 = function (){ alert('tool3') };
  return comp;
})(window);
```

一二三，根据这些简单的要求，就可以做一个自己的组件库了。

然后再将它放到自己的服务器上，就可以远程引入使用了，例如：
```html
<script src="https://www.starkl.win/demo/myComponents.js"></script>
<script type="text/javascript">
	myComponents.tool1('哈哈！我入门了模块化编程！');
</script>
```
