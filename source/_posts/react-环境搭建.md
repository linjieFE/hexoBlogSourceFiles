---
title: react 环境搭建
date: 2016-10-20 18:12:15
tags:
- react
---

## 1、node安装
首先，需要node环境支持，如果你还没有安装node.js，请移步[nodejs官网](https://nodejs.org/en/)。

## 2、初始化项目
```bash
npm init
```
回车即可 到最后，你的项目根目录会多一个package.json的文件。

## 3、添加项目依赖

接下来，我们打开package.json文件，给项目添加依赖包。当然，最简单的方法便是直接修改package.json文件，然后通过npm install安装依赖即可，但是考虑到依赖包的不断更新迭代，或许今后的不久就已经不是当前版本，为了能保证，我们每个看到这边文章的童鞋都是用的最新依赖包，我们直接通过npm install  **  --save-dev方式来安装我们依赖包。

首先，我们来安装dependencies：

```bash
npm install --save react react-dom lodash
```
按理来说，上面的命令并没有任何问题，但是小编却在这里遇到一个坑，那就是项目取名为react，然后再安装react的时候被拒绝
当然，解决方法就是重新命名，为了避免不必要的麻烦，还是推荐小伙伴们不要把项目名称命名成你要安装的包名，譬如：react，webpack等等。

泪的教训：如果你遇到上述的错误，请删掉该项目重新新建一个项目，因为无论如何也还是会报这个错误，包括重启也是于事无补。

再次安装react的时候，切忌一定要安装到生产依赖。
```bash
npm install --save-dev react
```
不出意外，应该是可以了，接下来我们就可以安装其他依赖。
```bash
npm install babel babel-core babel-loader babel-preset-es2015 babel-preset-react css-loader style-loader react-router webpack webpack-dev-server
```

```bash
npm install react-dom lodash --save
```
最后提醒一下，webpack和webpack-dev-server这两个包需要全局安装。
```bash
npm install -g webpack webpack-dev-server
```
到这里我们的依赖包就安装完毕。

以下便是package.json的最终代码：
```bash
{

"name": "react-demo",

"version": "1.0.0",

"description": "",

"main": "index.js",

"scripts": {

"test": "echo \"Error: no test specified\" && exit 1"

},

"author": "",

"license": "ISC",

"devDependencies": {

"babel": "^6.5.2",

"babel-core": "^6.13.2",

"babel-loader": "^6.2.4",

"babel-preset-es2015": "^6.13.2",

"babel-preset-react": "^6.11.1",

"css-loader": "^0.23.1",

"react": "^15.3.0",

"react-hot-loader": "^1.3.0",

"react-router": "^2.6.1",

"style-loader": "^0.13.1",

"webpack": "^1.13.1",

"webpack-dev-server": "^1.14.1"

},

"dependencies": {

"lodash": "^4.15.0",

"react-dom": "^15.3.0"

}

}

```
## 4、配置webpack
新建一个webpack.config.js文件。
```bash
var webpack = require("webpack")

var path = require("path")

module.exports = {

devtool: "inline-source-map",

entry: [

"webpack-dev-server/client?http://127.0.0.1:8080/",

"webpack/hot/only-dev-server",

"./app"

],

output: {

path: path.join(__dirname, "public"),

filename: "bundle.js"

},

resolve: {

modulesDirectories: ["node_modules", "app"],

extensions: ["", ".js"]

},

module: {

loaders: [

{

test: /\.jsx?$/,

exclude: /node_modules/,

loaders: ["react-hot", "babel?presets[]=react,presets[]=es2015"]

},

{

test: /\.css?$/,

exclude: /node_modules/,

loaders: ["style", "css"]

}
]

},

plugins: [

new webpack.HotModuleReplacementPlugin(),

new webpack.NoErrorsPlugin()

]

}

```
以上便是webpack的基本配置，具体的webpack配置以后会单独介绍。
## 5、项目文件
![](img/react_files.jpg)

index.html
```html
<!DOCTYPE html>

<html>

<head>

<title>React demo</title>

</head>

<body>

<div id="app"></div>

<script src="bundle.js"></script>

</body>

</html>

```
index.js
```html
import React from 'react'

import {render} from 'react-dom'render(

render(<div>hello wold</div>,document.getElementById("app"));
```

这个时候，一个简单的hello word就已经完成，运行如下命令

```bash
webpack-dev-server
```
使用浏览器打开http://127.0.0.1:8080就能看到hello world。这里的webpack-dev-server可以实时监测文件修改，已实时观看最终效果。

但是每次要预览效果，我们要输入这么一大串，难免有所不便，

打开package.json文件，找到scripts结点，更改如下所示：
```bash
"scripts": {

"test": "echo \"Error: no test specified\" && exit 1",

"dev":"webpack-dev-server"

}
```
这样，我们就只需要在命令行中输入：
```bash
npm run dev
```

当然，如果你嫌上面的步骤太过繁琐，你可以直接下载[github](https://github.com/swimly/react-demo)上面的代码，然后直接运行：
```bash
npm install
```


文／Swimly（简书作者）
原文链接：http://www.jianshu.com/p/0d7a70e39d2e
著作权归作者所有，转载请联系作者获得授权，并标注“简书作者”。

