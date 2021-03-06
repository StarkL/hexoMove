---
title: 放开那个IE8
date: 2017-11-01 09:47:42
tags:
---

#### 作为互联网公司，理性抛弃IE8

目标：用最低的成本，让我们的真实用户体验更好的服务

<br>

![](http://upload-images.jianshu.io/upload_images/4773358-93d1e9fae36a6391.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 用户体验：

如果为考虑公司1%的IE8用户而在一套代码里写入20%的兼容写法， 反而因为1%的用户，拖累了99%用户的体验。且为兼容IE8一些潮流的交互效果实现在一套代码里难度较高。日常开发常会因此放弃IE的交互效果。

![](http://upload-images.jianshu.io/upload_images/4773358-32b5f2ecaf909535.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 人员招聘：

17年擅长处理IE8兼容的，一般都至少工作2年了，而近些年前端技术激进，近年流行的 react vue基本是个入门前端都能挂着的标签，若一定要在一套代码里同时兼顾IE8，将会对前端人员的技术阅历有一定的考验。

另一方面，作为一个有偿工作人员，大多人(特别是注重成长的IT人员)都希望能在工作中成长，房价不涨即为降价的道理就被国家称为：软着陆。

为此，未来招聘难度和成本会上升。
  
开发成本：员工的时间就是公司的金钱，若为一套代码兼容IE8的开发方式，预计技术开发成本会相对于一套不考虑兼容IE8的成本高20%；
  <br>

![](http://upload-images.jianshu.io/upload_images/4773358-813346d316b6ef21.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 公司形象：

作为互联网金融公司，理应走在互联网技术前沿，以更美、更科技化的互联网面貌展示给受众。

多数使用IE8的用户是老一代用户，习惯了IE8的界面，固步自封在自己的习惯中，而之外有更美更舒适的环境，他们很难接触到。就像一直在农村生活的人，在没有太多束缚的情况下，到了大城市会很快爱上大城市的时尚潮流，而一直生活在大城市的人，到了农村，多会改造农村，无法直接接收农村的落后。这正是人们追逐更美好的一种本能。
    <br>
    
![](http://upload-images.jianshu.io/upload_images/4773358-784509790374ace0.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 技术界现状：

从H5的概念红遍网络以来，大部分人都通过现代浏览器逐渐认识到科技化前端带来的美好体验，就像是一直看2D电影的人第一次看到3D电影的感觉。稍有思想的前端人员，都愿意让自己、让公司能走在技术前沿，避免让那些99%见过H5美好的人看到自己公司的界面时，觉着很low，从而在页面包装方面影响对公司的好感。就像我们常用的app、手机系统，它们会尽可能的提示用户更新迭代自己的产品，以提高更优的交互体验和更符合公司业务需求的逻辑体验。

因此，网络充满前端对引导用户升级浏览器的各种方法：IE8就算能正常运行也让它变卡、抛弃IE8的任何交互效果、大家都称呼其为远古浏览器、友善弹窗提醒用户升级或切换浏览器，甚至像点融投资，直接从首页将IE8禁掉，只留下弹窗提醒用户升级或切换浏览器，还有全栈开发者们提出：偷偷的将用户浏览器升级、或者下载一个现代浏览器，将图标改为IE8图标。
<br>  

![](http://upload-images.jianshu.io/upload_images/4773358-10c8e9a2491f9763.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 技术选型

眼观现在公司项目：

1. 属于中小型项目，
2. 有部分复用性的模块，
3. 需要有一定的灵活增长空间，
4. 性能、代码逻辑结构、风格等有待优化
...

后台api + 前端渲染的开发方式，一般前端会选择 MVC 模式开发，启用模板渲染、分布式组件化开发，而IE8不支持Object.defineProperty，导致兼容它的模板渲染功能仅能选择较为落后(多数新生代前端都不熟悉)



  <br>

![](http://upload-images.jianshu.io/upload_images/4773358-d418e6a9f82c5a1e.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  

##### IE8在中国残存的原因：

2000年时，阿里为首的企业群将互联网在中国点爆，那时第一批尝鲜和希望通过互联网改革降低成本的是国内的公众服务系统，如：银行，这些系统庞大复杂，从那时的 xp系统 & IE6浏览器 已经开发出一套完整的系统。

　　随着2014年微软放弃xp系统的维护，国内公众服务系统不是一时半会儿就能更新完毕，不得不接手自主维护。银行系统等作为公众服务系统，是一定要涵盖任何一位国民的，所以对于IE6 7 8的少量用户群体，他们不能放弃，于是就存在这一直维护IE8的技术团队。

　　另一方面，xp系统也是公认的一代经典系统，还是有不少用户选择它做系统或虚拟机，就喜欢它的轻量、精简。

　　还有很真实的一种情况：2006——2010年买的台式电脑，低配，但对于多数老年人来说还能用；2010——2014年前后盛产的超极本，配置较低，仅作办公，有人会选择32位系统。上面两种情况下，IE8浏览器可以算是最佳体验了。

　　而这些人员并不见得是我们的投资用户。很高可能是我们用户的电脑上不仅仅有IE8，友善提醒用户切换浏览器对这1%的用户来说，可行性是相当高的。这样就能趋向于100%提高用户体验了。