---
title: 用nodeJS批量处理html文件
date: 2017-06-01 18:06:15
tags:
---
<br>

最近接手一个任务，给一个集团写一个静态站点，看了设计稿发现除了首页，其他页面的头部nav和底部footer都是一样的。如果写好粘贴赋值粘贴复制，多累啊！————于是想到一个需求：

<br>
<br>  

> 如何能让页面公用部分写一边，然后所有页面都能同步？  

<br>
作为前端出身，技术栈又比较少~ 第一个想到的就是nodeJS了！毕竟JS是前端特有唯一一门编程语言了！

---

<br>
<br>
首先，屡屡思路：  
<br>
<br>  

1. 目的是：公用部分单独写一份，然后页面只需要写除了公用部分之外的即可。
2. 前提是：如果出现异常，不能影响自己写的代码->所以要另外建立一个文件夹，用于存放node处理后的代码

<br>  
<br>  


接下来，开始node编写：  

<strong>section 1</strong>
<br>
一、 首先在新建文件夹下创建两个文件夹：  
> app（写代码的地方）
> public（生成代码的地方）
> handile.js（nodeJS文件）

目录结构如下：

![](https://ws3.sinaimg.cn/large/006tKfTcgy1fg886inyhzj30am03uq32.jpg)

<br>  
<br>  


二、 做到将app里的文件复制到public中  
　　解析功能
```javascript
//复制文件用到node的fs模块，用到readFile,writeFile功能，然而如果用这两个语法，它的原理是将文件内容读取保存在内存中，然后再一次性写入一个文件
//想想，这样就会遇到一个问题：如果文件比较大，一次性读取和写入，会不会很吃内存。
//就像是这里要从水龙头取水，将旁边一个无法移动的大缸接满，如果选择一次性接一缸水，再倒到水缸里，这显然是不明智的
//考虑这一点，这里要用Stream流信息处理方式，就是读着写着，像有水管一样，上面流着，下面接着，这样可行性就更高了,代码如下：
var fs = require('fs');
function copy(origin, aim) {
	fs.createReadStream(origin).pipe(fs.createWriteStream(aim));
}

//这里pipe方法就是这里举例中的水管
```
<br>
　　现在有了一个能连通数据流的水管了，这里要将水管入口接到水龙头上，出口接到大缸里  

<br>  
<br>  
　　但现在这里还遇到一个问题，app文件夹下是可能会有img css js等文件夹的，而这里的方法createReadStream只能读取指定的一个文件，
<br>  
<br>  
  
 <strong>所以这里要有一个遍历app和hbuild目录的方法，把app目录里所有文件的路径遍历出来，给copy方法的origin参数，再将app目录里文件的路径名字中app替换为hbuild，传给copy方法的aim参数，这样如果app文件夹下有多个文件，就相当于这里创造了多条水管，每个水管只负责一个文件信息流的传输</strong>

<br>
这里要用到readdir方法，这里选择同步读取，保证开水龙头，水管传输，水进入水缸的正确顺序，这样就不用promise来单独处理异步了。所以这里用readdirSync方法

```javascript
var path = require('path');
function travel(dir, callback) {
	fs.readdirSync(dir).forEach(function(file) {
		var pathname = path.join(dir, file); //将遍历到文件的路径给变量pathname
		
		if(fs.statSync(pathname).isDirectory()) {//判断是当前遍历到的文件是否是一个文件夹，如果是则进入文件夹再进行遍历：
			travelSync(pathname, callback);
		} else {
			callback(pathname);
		}
	})
}
```
<br>  
<br>

这样一个tranvel函数，需要两个参数，遍历的目录路径，和回调函数
这样这里就可以组合两个函数进行遍历和写入　　
<br>

```javascript
var fs = require('fs');
var path = require('path');

travelSync('./app', function(pathname) {
	if(pathname) {
		var publicPath = pathname.replace('app', 'public')
			copy(pathname, publicPath);
	}
})

function travelSync(dir, callback) {
	fs.readdirSync(dir).forEach(function(file) {
		var pathname = path.join(dir, file);
		console.log(pathname)
		if(fs.statSync(pathname).isDirectory()) {
			travelSync(pathname, callback);
		} else {
			callback(pathname);
		}
    console.log('文件复制成功！')
	});
}

function copy(src, dst) {
	fs.createReadStream(src).pipe(fs.createWriteStream(dst));
}

```
<br>

这里在app文件夹里新建几个目录和文件来做测试
<br>
![](https://ws4.sinaimg.cn/large/006tKfTcly1fghckwi0p5j307m0b8dg9.jpg)

header index footer 三个html文件里分别写：
```html
<!-- header.html -->
<header>
this is header!
</header>
```
```html
<!-- index.html -->
<div class="content">
this is index.html
</div>
```
```html
<!-- footer.html -->
<footer>
this is footer!
</footer>
```
<br>
然后在终端 cmd， 进入当前文件夹路径，执行 node handle.js　　
<br>
<br>

![](https://ws1.sinaimg.cn/large/006tKfTcly1fghcoh0xv5j306400s748.jpg)

<br>

然后再打开public文件夹，看下，发现：

<br>

![](https://ws3.sinaimg.cn/large/006tKfTcly1fghcp1s4bmj30hs036dg5.jpg)

哎！what？ 为什么css img js文件夹和里面的文件没有复制过来？
<br>
这是因为这里在遍历目录的时候，copy方法只实现了文件流信息的传输，但没有实现文件夹创建功能，所以这样运行会出现报错：  
<br>
![](https://ws2.sinaimg.cn/large/006tKfTcly1fgi61eege0j30ri024wev.jpg)

<br>

那么接下来就要写cipyDir方法：

```javascript
function copyDir(path) {
	fs.mkdir(path, function(err) { //mkdir方法创建文件夹只需要通过第一个参数告诉node路径和文件夹名即可创建，简单暴力
		if(!err) console.log(path+' 目录创建成功!')
	});
}
```

这里这里要修改一下travel方法，当travel遍历到文件夹的时候，在build文件夹下执行创建文件夹命令： 

```javascript
function travelSync(dir, callback) {
	fs.readdirSync(dir).forEach(function(file) {
		var pathname = path.join(dir, file);

		if(fs.statSync(pathname).isDirectory()) {
			callback(pathname,'dir'); //新增：遇到文件夹也执行回调，并传入一个字符串用于告知回调函数知道这是一个文件夹的pathname
			travelSync(pathname, callback);
		} else {
			callback(pathname,'file');//这里也添加告知回调函数文件类型的第二个参数'file'
		}
	});
}
```


接下来撸一遍现在的handle.js，是这样的：

```javascript

var fs = require('fs');
var path = require('path');


travelSync('./app', function(pathname, fileType) {
	if(pathname && fileType === 'file') {
		// app/header.html
		var publicPath = pathname.replace('app', 'public');
		copy(pathname, publicPath);
	} else if (pathname && fileType === 'dir') {
		// app/css
		var publicPath = pathname.replace('app', 'public');
		copyDir(publicPath)
	}
})

function travelSync(dir, callback) {
	fs.readdirSync(dir).forEach(function(file) {
		var pathname = path.join(dir, file);

		if(fs.statSync(pathname).isDirectory()) {
			callback(pathname,'dir');
			travelSync(pathname, callback);
		} else {
			callback(pathname,'file');
		}
	});
}

function copy(path, aimPath) {
	fs.createReadStream(path).pipe(fs.createWriteStream(aimPath));
}

function copyDir(path) {
	fs.mkdir(path, function(err) {
		if(!err) console.log(path+' 目录创建成功!')
	});
}
```

ok,现在再测试下复制功能是否能解决刚才无法复制文件夹的问题：命令行运行：node handle.js，打印出：

![](https://ws4.sinaimg.cn/large/006tKfTcly1fgi809xc8lj30fo046mxx.jpg)

再看public文件夹目录：

![](https://ws4.sinaimg.cn/large/006tKfTcly1fgi812xiskj30bq0dcmxr.jpg)

漂亮！现在复制功能已经达到。敲代码的，就是需要做好铺垫，才能快速行动~ 接下来进入关键时刻，处理文件的公用部分

---
<strong>section 2</strong>

<br>

这里模拟测试将app里的header footer 整合到index.html的 头 和 尾。 现在要用到readFile 和 writeFile语法，读取header footer里指定的内容，再写入index指定位置。

<br>

这里先想一个问题：如果文件比较大，内容比较复杂，如何才能准确的找到header footer里面指定的代码段， 然后准确的写入index指定的位置？先别看下面解决方案，两分钟，看自己能想出哪些方法

<br>
<br>
<br>
<br>
<br>
我暂想到相对好用的方法是：

1. 在header footer文件需要截取的代码段头尾添加一个标记行注释标签，同样，在index里面也放一个注释标签，告诉handle.js准确位置

也有一个不好用的：就是直接判断代码段开始标签，和结束标签，这样每次都要严格查询代码并做更改，其实挺麻烦的。

<br>
这里修改header  index  footer，添加标记注释，分别更改为：  

```html
<!-- header begin -->
<header>
	this is header!
</header>
<!-- header end -->
```  
```html
<!-- index begin -->
<div class="content">
	this is index.html
</div>
<!-- index end -->
```
```html
<!-- footer begin -->
<footer>
	this is footer!
</footer>
<!-- footer end -->
```
接下来在heandle.js里写header footer代码段截取功能，

```javascript
function getHeader() {
	fs.readFile('./app/header.html', function(err, html){
		var headerData = html.toString();
		// sliceStart只需要找到字符串的索引位置即可
		var sliceStart = headerData.indexOf('<!-- header begin -->');
		// sliceEnd找到字符串的位置后，加上字符串本身的长度再加2是一个换行符的
		var sliceEnd = headerData.indexOf('<!-- header end -->') + '<!-- header end -->'.length+2;
		var header = headerData.slice(sliceStart, sliceEnd);
		console.log(header)
	});
}
getHeader();
```

命令行运行node handle.js后 看到：  

![](https://ws1.sinaimg.cn/large/006tKfTcly1fgi9i75b9yj30hg04idge.jpg)   

这样就准确找到想要的代码段了。
由于还需要用同样的方法找到index和footer代码段，所以这里将getHeader方法封装成一个getAimCode方法：

```javascript
function getAimCode(headerPath, flagBegin, flagEnd) {
  fs.readFile(headerPath, function(err, html){
    if(!err) {
      var headerData = html.toString();
      // sliceStart只需要找到字符串的索引位置即可
      var sliceStart = headerData.indexOf(flagBegin);
      // sliceEnd找到字符串的位置后，加上字符串本身的长度再加2是一个换行符的
      var sliceEnd = headerData.indexOf(flagEnd) + flagEnd.length+2;
      var header = headerData.slice(sliceStart, sliceEnd);
      console.log(header)
    }
  });
}
//测试以下用getAimCode获取footer代码段
getAimCode('./app/footer.html', '<!-- footer begin -->', '<!-- footer end -->');
```  
封装函数最笨的办法：把可能会变动的内容，替换成参数变量，再通过函数调用，以参数的形式传进去，可以运行下上面代码，即可得到footer代码段，测试上面代码即可得到：

![](https://ws2.sinaimg.cn/large/006tKfTcly1fi7iyyrgl8j308i03ujrn.jpg)

现在能找到需要截取的代码段，接下来要进行代码段的准确插入：

其实插入代码段的基础原理，就是将目标文件信息读取出来，分为三段：
1. 插入位置之前的代码段 top
2. 插入header和footer之间的代码段 content 
3. 插入footer之后的代码段 bottom


然后只需要将代码段拼接成 top + header + content + footer + bottom 这么一个完整的代码块，再写入目标文件就完成了。

接下来对目标文件index进行分割：

为了明显看出index的分割效果，这里将index.html更改内容为：
 ```html
<html>
	<head>
		<title>index</title>
	</head>

<!-- index begin -->
<div class="content">
	this is index.html
</div>
<!-- index end -->

</html>
 ```  

回想下，从header.html中切出header代码段的时候，是找到
```html
<!-- header begin -->
和
<!-- header end -->
```
两个标记的索引位置，用String.slice(start,end)方法来切割的。那么index的三段一样要找到top content bottom三者的开始位置和结束位置。这里先默认index文件的内容被读取为：indexData，那么：
```html
1. top：是从文档的开始，开始位置就是索引值0，
2. 结束索引值就是index begin标记的开始，即indexData.indexOf('<!-- index begin -->');

3. content：开始索引值是indexData.indexOf('<!-- index begin -->');
4. 结束索引值是：indexData.indexOf('<!-- index end -->')+'<!-- index end -->'.length+2;

5. bottom：开始索引值是indexData.indexOf('<!-- index end -->')+'<!-- index end -->'.length+2,
6. bottom可以直接切到文档结尾，slice方法如果不传入第二个参数的话就默认直接从指定索引位置切割到结尾
```  

这样就可以写出函数：
```javascript
function cutIndex() {
  fs.readFile('./app/index.html', function(err, html) {
    if(!err) {
      var indexData = html.toString();
      var topStart = 0,
        topEnd = indexData.indexOf('<!-- index begin -->');
      var contentStart = indexData.indexOf('<!-- index begin -->'),
        contentEnd = indexData.indexOf('<!-- index end -->')+'<!-- index end -->'.length+2;
      var bottomStart = indexData.indexOf('<!-- index end -->')+'<!-- index end -->'.length+2;

      var top = indexData.slice(topStart, topEnd),
        content = indexData.slice(contentStart, contentEnd),
        bottom = indexData.slice(bottomStart);
      console.log(top+'top打印结束');
      console.log(content+'content打印结束');
      console.log(bottom+'\n bottom打印结束');
    }
  })
}
cutIndex();
```  
命令行里执行：node handle.js
打印出：

![](https://ws3.sinaimg.cn/large/006tKfTcly1fgirvxjl9xj30gg0ayq3y.jpg)  

现在得到了header footer top content bottom，接下来要做拼接并的功能。  

将我们的代码组装起来成为：
```javascript  
fs.readFile('./app/header.html', function(err, html){
	if(!err) {
		var headerData = html.toString();
		// sliceStart只需要找到字符串的索引位置即可
		var sliceStart = headerData.indexOf('<!-- header begin -->');
		// sliceEnd找到字符串的位置后，加上字符串本身的长度再加2是一个换行符的
		var sliceEnd = headerData.indexOf('<!-- header end -->') + '<!-- header end -->'.length+2;
		var header = headerData.slice(sliceStart, sliceEnd);
	}

	fs.readFile('./app/footer.html', function(err, html){
		if(!err) {
			var footerData = html.toString();
			// sliceStart只需要找到字符串的索引位置即可
			var sliceStart = footerData.indexOf('<!-- footer begin -->');
			// sliceEnd找到字符串的位置后，加上字符串本身的长度再加2是一个换行符的
			var sliceEnd = footerData.indexOf('<!-- footer end -->') + '<!-- footer end -->'.length+2;
			var footer = footerData.slice(sliceStart, sliceEnd);
		}
		
		fs.readFile('./app/index.html', function(err, html) {
			if(!err) {
				var indexData = html.toString();
				var topStart = 0,
					topEnd = indexData.indexOf('<!-- index begin -->');
				var contentStart = indexData.indexOf('<!-- index begin -->'),
					contentEnd = indexData.indexOf('<!-- index end -->')+'<!-- index end -->'.length+2;
				var bottomStart = indexData.indexOf('<!-- index end -->')+'<!-- index end -->'.length+2;

				var top = indexData.slice(topStart, topEnd),
					content = indexData.slice(contentStart, contentEnd),
					bottom = indexData.slice(bottomStart);
			}
			console.log(top+header+content+footer+bottom);
		})
	})
})
```  
执行代码就可以得到：

![](https://ws2.sinaimg.cn/large/006tNbRwly1fgit2euk34j30je0gedhu.jpg)  

最终，结合section1，handle.js要完成：
1. 先将app文件夹的目录结构复制到public文佳夹下
2. 对文件切割完毕，重组合，再写入public文件夹下的index.html中

基础版最终代码如下：
```javascript
travelSync('./app', function(pathname, fileType) {
	if(pathname && fileType === 'file') {
		// app/header.html
		var publicPath = pathname.replace('app', 'public');
		copy(pathname, publicPath);
	} else if (pathname && fileType === 'dir') {
		// app/css
		var publicPath = pathname.replace('app', 'public');
		copyDir(publicPath)
	}

	fs.readFile('./app/header.html', function(err, html){
		if(!err) {
			var headerData = html.toString();
			// sliceStart只需要找到字符串的索引位置即可
			var sliceStart = headerData.indexOf('<!-- header begin -->');
			// sliceEnd找到字符串的位置后，加上字符串本身的长度再加2是一个换行符的
			var sliceEnd = headerData.indexOf('<!-- header end -->') + '<!-- header end -->'.length+2;
			var header = headerData.slice(sliceStart, sliceEnd);
		}

		fs.readFile('./app/footer.html', function(err, html){
			if(!err) {
				var footerData = html.toString();
				// sliceStart只需要找到字符串的索引位置即可
				var sliceStart = footerData.indexOf('<!-- footer begin -->');
				// sliceEnd找到字符串的位置后，加上字符串本身的长度再加2是一个换行符的
				var sliceEnd = footerData.indexOf('<!-- footer end -->') + '<!-- footer end -->'.length+2;
				var footer = footerData.slice(sliceStart, sliceEnd);
			}
			
			fs.readFile('./app/index.html', function(err, html) {
				if(!err) {
					var indexData = html.toString();
					var topStart = 0,
						topEnd = indexData.indexOf('<!-- index begin -->');
					var contentStart = indexData.indexOf('<!-- index begin -->'),
						contentEnd = indexData.indexOf('<!-- index end -->')+'<!-- index end -->'.length+2;
					var bottomStart = indexData.indexOf('<!-- index end -->')+'<!-- index end -->'.length+2;

					var top = indexData.slice(topStart, topEnd),
						content = indexData.slice(contentStart, contentEnd),
						bottom = indexData.slice(bottomStart);
				}
				var indexChunk = top+header+content+footer+bottom;
				fs.writeFile('./public/index.html', indexChunk, function(err) {
					if(!err) console.log('文件处理成功！')
				})
			})
		})
	})
})
```  

再次在命令行里执行：node handle.js
终端输出：

![](https://ws1.sinaimg.cn/large/006tNbRwly1fgitccgnncj30bo06agma.jpg)  

再打开public/index.html，就变成了：  

![](https://ws4.sinaimg.cn/large/006tNbRwly1fgitd3zlxzj30ie0l4tau.jpg)  

这里node命令是异步处理，所以出现代码嵌套非常多，稍微改动就成了传说中的回调地狱~   

后续更新promise优化版。