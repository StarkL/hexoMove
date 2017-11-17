---
title: 第一次webpack使用流程
date: 2017-06-01 18:06:58
tags:
---

### 开发环境下
1. nodeJS（提供ES6）环境下
2. 用npm init 生成package.json用于配置依赖包信息
3. 创建两个文件夹After Before，一个用于存放自己编写代码，一个用于webpack生成文件的存放
4. webpack执行是根据一个入口文件进行压缩配置，在输出到一个出口文件里，所以在Before文件夹下创建一个main.js作为入口文件；所有需要输出的代码最终都要引入到这个文件中 import React from 'React'; import './main.css';
5. 根据实际开发需求，渐进式的安装所需的依赖包，例如：npm install json-loader --save；npm install css-loader, style-loader --save；逐步通过保存本地式(--save)安装的依赖包会被自动添加到package.json中
6. 开发结束，需要生成最终文件，执行npm webpack将文件合并生成到After文件夹中的指定文件
7. 在After文件夹中创建index.html文件，并引入最终生成的js文件即可；After下的文件即为最终文件