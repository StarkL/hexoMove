---
title: css3里的贝塞尔运动曲线
date: 2017-08-23 10:26:42
tags:
---


贝塞尔曲线，常用在css3中 <b>transition-timing-function</b> 的值里；

而这个属性是控制css3动画的运动轨迹

![](https://ws2.sinaimg.cn/large/006tKfTcgy1fiteay7136j30iq0j2q3t.jpg)

上图的贝塞尔曲线是经典的 ease-in 曲线，x轴表示随着时间匀速前进，y轴表示随着时间推移运动距离的百分比，x y的最大值都为 <b>1<b>

---

#### 贝塞尔值

贝塞尔运动曲线有两个值，如ease-in的贝塞尔值：

```css
transition:all 1s cubic-bezier(.42,0,1,1);
```

可以看到，cubic-bezier 中有4个值，这4个值代表的是控制两个曲线的两个圆点在坐标中的位置。
0.42,0 表示的是上图中<b>红色圆点</b>的坐标， 1,1表示的是<b>绿色圆点</b>的坐标。由此组成了一个完整的贝塞尔值。

理解了贝塞尔值的意思，接下来介绍这两个圆点是干什么的（做过ps flash之类设计的应该能猜到）。


#### 控制锚点

专业术语也难说得通俗易懂，就还用 <b>红点 绿点</b> 来说吧。

这两个圆点的位置，-> 决定了这个曲线的形状，-> 形状又决定了运动轨迹的变化，-> 从而形成我们看到的曲线运动

![](https://ws3.sinaimg.cn/large/006tKfTcly1fitfdi3byzg308007wq2z.gif)

这两个圆点是如何控制曲线的， [点我](http://cubic-bezier.com/#.17,.67,.83,.67)，自己动。

![](https://ws2.sinaimg.cn/large/006tKfTcgy1fitn5avmhlg308x0cbwx1.gif)

操控原理上用了高数中的切线，
圆点带动的那条线，就是这两条线连接处的一个切线。

就以此图为例：

![](https://ws2.sinaimg.cn/large/006tKfTcgy1fiteay7136j30iq0j2q3t.jpg)

通过两个圆点操控出的这条曲线，意思是：
* 随着时间匀速前进，
* 动画的运动是先慢慢移动，
* 再逐渐加快移动。

就是常用的 ease-in 动画

贴出一段代码，做做测试就知道了：

点击[RunJS](http://runjs.cn/code/pjt4uto4)，鼠标移入小球，即可观看。

根据自己调好的贝塞尔值，进行测试。


---
这里遇到一个问题： 为何曲线运动轨迹，会出现折线的情况：

![](https://ws3.sinaimg.cn/large/006tKfTcgy1fitg4k3bvej30dc0ceglv.jpg)

