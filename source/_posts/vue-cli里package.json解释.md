---
title: vue-cli里package.json解释
date: 2017-05-26 17:55:18
tags:	简单解释下vue-cli的项目最外层都引入了什么组件
---

这里简单解释下vue-cli的项目最外层都引入了什么组件 做什么用

```json
{ //这个代码块里的都是项目描述，是创建者自定义的
  "name": "qqmusic",  //项目名称
  "version": "1.0.0",   //项目版本
  "description": "qq music app by vue", //项目描述
  "author": "yangbo", //项目作者
  "private": true, //是否为私有
  "scripts": { //这里是命名一些变量对应什么指令
    "dev": "node build/dev-server.js", //运行dev 相当于运行node build/dev-server.js
    "start": "node build/dev-server.js", //同上
    "build": "node build/build.js", //同上
    "lint": "eslint --ext .js,.vue src" //同上
  },
  "dependencies": { //这个代码块描述此 项目依赖 的插件；下面两个做什么的自行搜索
    "vue": "^2.2.6", 
    "vue-router": "^2.3.1"
  },
  "devDependencies": { //这个代码块里描述的是此项目 运行依赖 插件；注意根上面项目依赖的区分
    "autoprefixer": "^6.7.2", //这英文翻译过来就是：自动 预 修复者，专业称呼知道的可以回复下；它是一个css前缀处理工具，有了它我们就不用写很多前缀了
    "babel-core": "^6.22.1", //babel是es6编译工具，将es6 7语法编译成es5语法，以兼容不支持es6 7的浏览器，这个是babel的核心组件
    "babel-eslint": "^7.1.1", //eslint是一个约束编码者的代码风格工具，这个是babel版本的eslint，如果那里写的不符合eslint的要求，就会报错，练习这个建议用熟练的项目来练习
    "babel-loader": "^6.2.10", //这是babel的加载组件，有了它就可以让babel和webpack对我们的js文件进行处理转换
    "babel-plugin-transform-runtime": "^6.22.0", //babel用来修复自己转换不足的一个组件 https://segmentfault.com/q/1010000005596587?from=singlemessage&isappinstalled=1
    "babel-preset-env": "^1.3.2", //智能识别当前运行环境，不支持es6 7才进行转换，在高性能浏览器上会保留高性能的es6 7语法
    "babel-preset-stage-2": "^6.22.0", //stage0 1 2 3是babel编译范围的一个标准，2版本适合开发使用 http://www.cnblogs.com/chris-oil/p/5717544.html
    "babel-register": "^6.22.0", //官方介绍它可以让babel-core自动钩到我们需要编译的地方，懂得请回复，方便大家
    "chalk": "^1.1.3", // 代码的颜色是它帮忙实现的
    "connect-history-api-fallback": "^1.3.0", //解决单页面应用刷新和直接通过域名+参数访问时候出现404的不足 https://github.com/bripkens/connect-history-api-fallback
    "copy-webpack-plugin": "^4.0.1", //webpack的一个文件和目录复制工具，项目部分路径问题的解决者
    "css-loader": "^0.28.0", //css加载组件，默认css不支持@import ../path/test.css 这种在一个css中引入另一个css的方式，而有了它，就可以这样引入了 http://cache.baiducontent.com/c?m=9f65cb4a8c8507ed4fece763105392230e54f73f7e88885468d4e419ce3b46031437bae872750d5592846b6777f1140fbca77666725e60e19499c90bcabae23f2ffe30350042db12448459&p=8c769a47c09f05ff57ee957e610a86&newp=8b2a97118d9159ff57ee957e500793231610db2151d7d5166b82c825d7331b001c3bbfb42324130fd3c0786707ac4b59eff23570310221a3dda5c91d9fb4c57479dc7c6668&user=baidu&fm=sc&query=css-loader&qid=b877345400022555&p1=3
    "eslint": "^3.19.0", //这是官方eslint，用于统一代码风格的工具，写的不符合规范就给报错，再次建议，在自己有把握的熟练项目里练习它比较好
    "eslint-friendly-formatter": "^2.0.7", //个人理解为一个修复eslint导致编辑器打开文件异常的组件 https://www.npmjs.com/package/eslint-friendly-formatter
    "eslint-loader": "^1.7.1", //eslint加载器，类似babel-loder功能
    "eslint-plugin-html": "^2.0.0", //
    "eslint-config-standard": "^6.2.1", //eslint的一个js标准样式配置组件，用于配置如何规定js代码规范的 http://npm.taobao.org/package/eslint-config-standard
    "eslint-plugin-promise": "^3.4.0", //不清楚：难道是针对js的promise功能单独搞的组件？ 请高手回复 https://www.npmjs.com/package/eslint-plugin-promise
    "eslint-plugin-standard": "^2.0.1", //eslint标准生产组件 https://www.npmjs.com/package/eslint-plugin-standard
    "eventsource-polyfill": "^0.9.6", //不清楚：挺常见的单词，难道是解决跨域请求问题的？ 请高手回复 https://www.npmjs.com/package/eventsource
    "express": "^4.14.1", //一个nodeJS常用的框架，超好用，集成很多常用功能，类似jquery的能力
    "extract-text-webpack-plugin": "^2.0.0", //该组件可以解决webpak将css样式打包到js中出现样式错乱的问题 https://github.com/webpack-contrib/extract-text-webpack-plugin http://www.cnblogs.com/dyx-wx/p/6529447.html
    "file-loader": "^0.11.1", //webpack的文件加载组件，解决路径问题的关键组件，
    "friendly-errors-webpack-plugin": "^1.1.3", //报错的有个友好组件，当有报错，会将error信息显示在可视区域
    "html-webpack-plugin": "^2.28.0", //帮助我们生成最终html文件的一个组件 https://www.npmjs.com/package/html-webpack-plugin
    "http-proxy-middleware": "^0.17.3", //http代理中间件，这里用于辅助解决路径问题 https://www.npmjs.com/package/http-proxy-middleware
    "webpack-bundle-analyzer": "^2.2.1", //很有意思的一个组件，用于优化压缩和查找错误模块 https://www.npmjs.com/package/webpack-bundle-analyzer
    "semver": "^5.3.0", //不清楚：难道是版本规范工具？ http://semver.org/lang/zh-CN/
    "shelljs": "^0.7.6", //为nodeJS提供的一个仿UNIX的shell命令的组件 https://www.npmjs.com/package/shelljs
    "opn": "^4.0.2", //为nodeJS提供的一个跨平台打开文件的方法，
    "optimize-css-assets-webpack-plugin": "^1.3.0", //
    "ora": "^1.2.0", //不清楚：难道是提供一个识别参数文件的后缀功能？ https://zhidao.baidu.com/question/581436199.html
    "rimraf": "^2.6.0", //为nodeJS提供的一个UNIX命令，rm -rf 移除文件的功能
    "url-loader": "^0.5.8", //功能同file-loader，不过在它基础上增加了小于8kb的图片直接转换成base64码写进js，减少图片请求，节约资源
    "vue-loader": "^11.3.4", // vue的加载工具，它允许我们使用vue的格式写文件 https://github.com/vuejs/vue-loader
    "vue-style-loader": "^2.0.5", //webpack的vue.js样式加载模块 http://npm.taobao.org/package/vue-style-loader
    "vue-template-compiler": "^2.2.6", //vue编译模板，用于将vue2.0+编译成可供浏览器渲染的常规正常函数 https://npm.taobao.org/package/vue-template-compiler
    "webpack": "^2.3.3", //一款打包工具，还有其他功能，例如路径处理等
    "webpack-dev-middleware": "^1.10.0", //本地改动，webpack相关功能都会实时刷新 https://github.com/webpack/webpack-dev-middleware
    "webpack-hot-middleware": "^2.18.0", //开发时特有的一个中间件，我们本地测试文件的改动都会实时在内存缓存里存储，并没有写入磁盘；
    "webpack-merge": "^4.1.0" //webpack合并文件的组件，我们写很多js文件，最后webpack会将它们合并成一个js，减少网络请求次数 https://www.npmjs.com/package/webpack-merge
  },
  "engines": { // 引擎相关的内容
    "node": ">= 4.0.0", //要求node版本要大于指定版本，否则不给你运行~
    "npm": ">= 3.0.0" //要求npm版本大于指定版本
  },
  "browserslist": [ // 配置浏览器的信息查询范围，这些信息将给Autoprefixer babel-env-preset eslint-plugin-compat这些组件来使用 https://www.npmjs.com/package/browserslist
    "> 1%", //
    "last 2 versions", //每种浏览器的最近的两个版本
    "not ie <= 8" //IE8及以下版本不查询
  ]
}
```