---
title: 通过url参数控制页面
date: 2017-05-27 15:08:38
tags:
---

今天写一个静态站的时候，遇到一个问题：站点页面之间可以通过a链接相互跳转，但是nav里有部分导航直接导航到页面的某个位置，甚至根据多个导航指向一个页面的不同功能区。

应用场景：用户只是点击了nav的一个链接，希望打开页面就能看到自己想看到的，不希望在打开页面后再进行其他操作才能到达目的地。

这样的话，就需要在页面加载完毕的时候就已经是用户想要的画面。于是想到一个办法：

```
	直接在a链接里href上带上约定参数param，然后在打开页面的时候js获取这个参数，根据参数不同执行不同的内容即可
	
```

这里就在html里写

```html
	<a href="pagename=dynamic">内容内容</a>
```

javascript里：  

```javascript
	var url = location.href;
	
	var sliceStart = url.indexOf('pagname');
	var pagename = url.slice(sliceStart); //pagename=dynamic
	
	//这里考虑到如果后台嵌入的时候可能会在url里嵌入其他参数，所以要做以下处理
	//首先url里多参数的形式是：https://www.baidu.com?cool=yes&reach=no&strong=yes
	//所以需要根据&符号来对刚才得到的pagename进行再次切割
	
	if(pagename.match('&')) {
		var sliceEnd = pagename.indexOf('&');
		pagneame = pagename.slice('pagename='.lenth, sliceEnd)	; //dynamic
	}
	
	//这样就得到了用户点击时传过来的参数，通过参数再控制页面该做什么就ok了。
	
```

如果读者有其他方法，敬请分享。