---
title: gulp学习笔记
date: 2016-07-12 11:51:33
categories: 学习笔记
tags:
- gulp
- 学习笔记
---
Gulp前端自动化：Gulp的高度集成化开发环境，释放了前端开发中大量时间，如css压缩、js压缩、错误检查、合并js、压缩图片、压缩html、模块构造等，只要你能想到的基本都可以通过Gulp插件去实现。

## 前端自动化的目的
在一个项目过程中，重复而枯燥的工作太多了……绳命就这样浪费了。
我们需要一个自动化的工作流程，让我们更专注于coding，而不是coding外的繁琐工作。于是Gulp应运而生。可以想像，如果在node环境下，一行命令搞定一个场景，So Cool…
然而通过了解，显然可看出Gulp
### 功能

1.版本控制
2.检查JS
3.图片合并
4.压缩CSS
5.压缩JS
6.编译SASS

目前最知名的构建工具： Gulp、Grunt、NPM + Webpack；

. grunt是前端工程化的先驱

. gulp更自然基于流的方式连接任务

. Webpack最年轻，擅长用于依赖管理，配置稍较复杂

. 推荐使用Gulp，Gulp基于nodejs中stream，效率更好语法更自然,不需要编写复杂的配置文件

## 安装前准备：
Gulp是基于 Node.js的，需要要安装 Node.js

为了确保依赖环境正确，我们先执行几个简单的命令检查。
``` bash
$ node -v

v5.3.0

Node是一个基于Chrome JavaScript V8引擎建立的一个解释器
检测Node是否已经安装，如果正确安装的话你会看到所安装的Node的版本号

接下来看看npm，它是 node 的包管理工具，可以利用它安装 gulp 所需的包

$ npm -v

3.3.12

这同样能得到npm的版本号，装 Node 时已经自动安装了npm

```
## 开始全局安装Gulp

``` bash
$ npm install -g gulp
```
``` bash
$ gulp -v

[18:39:18] CLI version 3.9.1

得到gulp的版本号，确认安装成功

```
## 创建工程

``` bash

TestProject     (项目名称)
|–.git               通过git进行版本控制,项目自动生成这个文件
|–node_modules       组件包目录
|–dist               发布环境（编译自动生成的）
    |–css                 样式文件(style.css style.min.css)
    |–img                 图片文件(压缩图片\合并后的图片)
    |–js                  js文件(main.js main.min.js)
    |–index.html          静态页面文件(压缩html)

|–src                开发环境
    |–sass                sass文件
    |–images              图片文件
    |–js                  js文件
    |–index.html          静态文件
|–gulpfile.js        gulp配置文件
|–package.json       依赖模块json文件,在项目目录下npm install会安装项目所有的依赖模块，简化项目的安装程序

```
## 创建package.json

我们先使用npm init来创建类似Nuget package的package.config一样的文件，这样我们就知道项目依赖哪些插件，而且我们不需要把插件提交到代码库，其它程序员只需要使用 npm install 就可以安装所有配置的插件

``` bash
$ npm init
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

See `npm help json` for definitive documentation on these fields
and exactly what they do.

Use `npm install <pkg> --save` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
name: (test) test                  //名称
version: (1.0.0) 1.0.0             //版本
description: test description      //描述
entry point: (index.js)            //
test command:                      //测试代码
git repository:                    //Git版本库
keywords:                          //关键词
author: luuman                     //作者
license: (ISC)                     //协议
About to write to F:\Gulp\test\package.json:

{
  "name": "test",
  "version": "1.0.0",
  "description": "test description",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "luuman",
  "license": "ISC"
}

Is this ok? (yes)

```
## 我们要进去到项目的根目录再安装一遍

``` bash
$ npm install gulp --save-dev

```
## 新建gulpfile.js文件
我们将要使用Gulp插件来完成我们以下任务：

. sass的编译（gulp-sass）
. 自动添加css前缀（gulp-autoprefixer）
. 压缩css（gulp-minify-css）
. js代码校验（gulp-jshint）
. 合并js文件（gulp-concat）
. 压缩js代码（gulp-uglify）
. 压缩图片（gulp-imagemin）
. 自动刷新页面（gulp-livereload）
. 图片缓存，只有图片替换了才压缩（gulp-cache）
. 更改提醒（gulp-notify）

### 安装这些插件需要运行如下命令：

```bash
npm install gulp-jshint gulp-sass gulp-less gulp-autoprefixer gulp-clean-css gulp-imagemin gulp-notify gulp-cache imagemin-pngquant gulp-livereload gulp-concat gulp-uglify  gulp-rename  gulp-babel del --save-dev
```
gulp功能模块的文件会放在项目所在的目录的./node_modules 下
更多插件可以看这里[gulpjs.com/plugins/](http://gulpjs.com/plugins/)

接着我们要创建一个gulpfile.js在根目录下，gulp只有四个API： task，watch，src，和 dest

``` bash
task--这个API用来创建任务，在命令行下可以输入 gulp test 来执行test的任务。
watch--这个API用来监听任务。
src--这个API设置需要处理的文件的路径，可以是多个文件以数组的形式[main.scss, vender.scss]，也可以是正则表达式/**/*.scss。

dest--这个API设置生成文件的路径，一个任务可以有多个生成路径，一个可以输出未压缩的版本，另一个可以输出压缩后的版本。

```
### 加载插件：

``` bash
// 引入gulp
var gulp = require('gulp');

// 引入组件
var jshint = require('gulp-jshint'); //检查js代码校验
var sass = require('gulp-sass'); //编译Sass
var less = require('gulp-less'); //编译less
var autoprefixer = require('gulp-autoprefixer');
var cleancss = require('gulp-clean-css');
var imagemin = require('gulp-imagemin'); //压缩图片
var notify = require('gulp-notify'); //处理报错
var cache = require('gulp-cache'); //图片缓存，只有图片替换了才压缩
var pngquant = require('imagemin-pngquant'); //深度压缩图片
var livereload = require('gulp-livereload'); //服务器控制客户端同步刷新（需配合chrome插件LiveReload及tiny-lr）
var concat = require('gulp-concat'); //合并js
var uglify = require('gulp-uglify'); //uglify 组件（用于压缩 JS）
var rename = require('gulp-rename'); //重命名
var babel = require('gulp-babel'); //编译es6
var del = require('del');//清除文件
var plumber = require('gulp-plumber');//当发生异常时提示错误

```
最后是我自己设置的项目文件路径

|--/src/--------开发环境
|--/desc/-------生成目录
|--/plugin/-----插件存放目录
|--gulpfile.js

###  编译sass、自动添加css前缀和压缩
首先我们编译sass，添加前缀，保存到我们指定的目录下面，还没结束，我们还要压缩，给文件添加 .min 后缀再输出压缩文件到指定目录，最后提醒任务完成了：

``` bash
// Styles任务
gulp.task('styles', function() {
    //编译sass
    return gulp.src(['src/less/*.less','src/css/*.css'])
    //css 合并
    .pipe(concat('all.css'))
    //当发生异常时提示错误 确保本地安装gulp-notify和gulp-plumber
    .pipe(plumber({ errorHandler: notify.onError('Error: <%= error.message %>') }))
    .pipe(less())
    .pipe(gulp.dest('src/css'))
    //添加前缀
    .pipe(autoprefixer('last 2 version', 'safari 5', 'ie 8', 'ie 9', 'opera 12.1', 'ios 6', 'android 4'))
    //保存未压缩文件到我们指定的目录下面
    .pipe(gulp.dest('src'))
    //给文件添加.min后缀
    .pipe(rename({ suffix: '.min' }))
    //压缩样式文件
    .pipe(cleancss())
    //输出压缩文件到指定目录
    .pipe(gulp.dest('dist/css'))    
    //提醒任务完成
    .pipe(notify({ message: 'Styles task complete' }));
});
```
### js代码校验、合并和压缩

``` bash
// 合并，压缩js文件
// 找到 js/ 目录下的所有 js 文件，压缩，重命名，最后将处理完成的js存放在 dist/js/ 目录下
gulp.task('scripts', function() {
    return gulp.src('src/js/*.js') //可配置你需要检查脚本的具体名字。
        //js代码校验
        .pipe(jshint())
        .pipe(jshint.reporter('default'))
        //js代码合并
        .pipe(concat('all.js'))
        //压缩脚本文件
        .pipe(uglify())
        //给文件添加.min后缀
        .pipe(rename({ suffix: '.min' }))
        //压缩脚本文件
        .pipe(uglify())
        //输出压缩文件到指定目录
        .pipe(gulp.dest('dist/js'))
    //提醒任务完成
    .pipe(notify({ message: 'Scripts task complete' }))
    console.log('gulp task is done'); //自定义提醒信息
});

```
### 图片压缩
``` bash
//压缩图片
gulp.task('testImagemin', function() {
   return gulp.src('src/img/*.{png,jpg,gif,ico}')
        .pipe(cache(imagemin({
            progressive: true,
            svgoPlugins: [{ removeViewBox: false }],
            use: [pngquant()]
        })))
        .pipe(gulp.dest('dist/img'))
        .pipe(notify({ message: 'Images task complete' }));
});
```

### 事件监听
``` bash
//监听 Watch
gulp.task('testWatch', function() {
    // Watch .less files
    gulp.watch('src/**/*.less', ['testLess']); //当所有less文件发生改变时，调用testLess任务
    // Watch image files
    gulp.watch('src/**/*.img', ['testImagemin']);
    // Watch .js files
    gulp.watch('js/*.js', ['scripts']);
    // Create LiveReload server
   livereload.listen();
   // Watch any files in assets/, reload on change
   gulp.watch(['dist/**']).on('change', livereload.changed);
});
```
### 清除文件

``` bash
//clean 清除文件 在任务执行前，最好先清除之前生成的文件
gulp.task('clean', function(cb) {
    del(['dist/css', 'dist/js', 'dist/img'], cb)
});
```
### 默认任务
``` bash
// 默认任务 Default task
gulp.task('default', ['clean'],function() {
    gulp.start('help','testLess', 'scripts', 'testImagemin');
    gulp.src('src/js/*.js')
        .pipe(babel({
            presets: ['es2015']
        }))
        .pipe(gulp.dest('src/bjs'))
});
```

## 其它插件
``` bash
// 检查js脚本的任务
gulp.task('lint', function() {
  return gulp.src('js/*.js') //可配置你需要检查脚本的具体名字。
        .pipe(jshint())
        .pipe(jshint.reporter('default'));
});

// 编译Sass
gulp.task('sass', function() {
  return gulp.src('./scss/*.scss')
        .pipe(sass())
        .pipe(gulp.dest('css/'));//dest()写入文件
});

// 编译less
gulp.task('testLess', function() {
  return gulp.src('src/less/*.less')
        //当发生异常时提示错误 确保本地安装gulp-notify和gulp-plumber
        .pipe(plumber({ errorHandler: notify.onError('Error: <%= error.message %>') }))
        .pipe(less())
        .pipe(gulp.dest('src/css'));
});
```

``` bash
npm list | gulp <模糊插件匹配>
```
### gulp 最终配置文件
``` bash
// 引入gulp
var gulp = require('gulp');

// 引入组件
var jshint = require('gulp-jshint'); //检查js代码校验
var sass = require('gulp-sass'); //编译Sass
var less = require('gulp-less'); //编译less
var autoprefixer = require('gulp-autoprefixer');
var cleancss = require('gulp-clean-css');
var imagemin = require('gulp-imagemin'); //压缩图片
var notify = require('gulp-notify'); //处理报错
var cache = require('gulp-cache'); //图片缓存，只有图片替换了才压缩
var pngquant = require('imagemin-pngquant'); //深度压缩图片
var livereload = require('gulp-livereload'); //服务器控制客户端同步刷新（需配合chrome插件LiveReload及tiny-lr）
var concat = require('gulp-concat'); //合并js
var uglify = require('gulp-uglify'); //uglify 组件（用于压缩 JS）
var rename = require('gulp-rename'); //重命名
var babel = require('gulp-babel'); //编译es6
var del = require('del');//清除文件
var plumber = require('gulp-plumber');//当发生异常时提示错误

// 默认任务 Default task
gulp.task('default', ['clean'] ,function() {
    gulp.start('help','styles', 'scripts', 'testImagemin');
    gulp.src('src/js/*.js')
        .pipe(babel({
            presets: ['es2015']
        }))
        .pipe(gulp.dest('src/bjs'))
});

// help task
gulp.task('help', function() {
    console.log("gulp build  文件打包");
    console.log("gulp watch  文件监控");
    console.log("gulp help  gulp参数说明");
    console.log("gulp server  测试sever");
    console.log("gulp -p  生产环境");
    console.log("gulp -d  开发环境");
    console.log("gulp -m <module>  部分模块打包（默认全部打包）");
});

// 检查js脚本的任务
gulp.task('lint', function() {
  return gulp.src('js/*.js') //可配置你需要检查脚本的具体名字。
        .pipe(jshint())
        .pipe(jshint.reporter('default'));
});

// 编译Sass
gulp.task('sass', function() {
  return gulp.src('./scss/*.scss')
        .pipe(sass())
        .pipe(gulp.dest('css/'));//dest()写入文件
});

// 编译less
gulp.task('testLess', function() {
  return gulp.src('src/less/*.less')
        //当发生异常时提示错误 确保本地安装gulp-notify和gulp-plumber
        .pipe(plumber({ errorHandler: notify.onError('Error: <%= error.message %>') }))
        .pipe(less())
        .pipe(gulp.dest('src/css'));
});

// Styles任务
gulp.task('styles', function() {
    //编译sass
    return gulp.src(['src/less/*.less','src/css/*.css'])
    //css 合并
    .pipe(concat('all.css'))
    //当发生异常时提示错误 确保本地安装gulp-notify和gulp-plumber
    .pipe(plumber({ errorHandler: notify.onError('Error: <%= error.message %>') }))
    .pipe(less())
    .pipe(gulp.dest('src/css'))
    //添加前缀
    .pipe(autoprefixer('last 2 version', 'safari 5', 'ie 8', 'ie 9', 'opera 12.1', 'ios 6', 'android 4'))
    //保存未压缩文件到我们指定的目录下面
    .pipe(gulp.dest('src'))
    //给文件添加.min后缀
    .pipe(rename({ suffix: '.min' }))
    //压缩样式文件
    .pipe(cleancss())
    //输出压缩文件到指定目录
    .pipe(gulp.dest('dist/css'))    
    //提醒任务完成
    .pipe(notify({ message: 'Styles task complete' }));
});

//压缩图片
gulp.task('testImagemin', function() {
   return gulp.src('src/img/*.{png,jpg,gif,ico}')
        .pipe(cache(imagemin({
            progressive: true,
            svgoPlugins: [{ removeViewBox: false }],
            use: [pngquant()]
        })))
        .pipe(gulp.dest('dist/img'))
        .pipe(notify({ message: 'Images task complete' }));
});

// 合并，压缩js文件
// 找到 js/ 目录下的所有 js 文件，压缩，重命名，最后将处理完成的js存放在 dist/js/ 目录下
gulp.task('scripts', function() {
    return gulp.src('src/js/*.js') //可配置你需要检查脚本的具体名字。
        //js代码校验
        .pipe(jshint())
        .pipe(jshint.reporter('default'))
        //js代码合并
        .pipe(concat('all.js'))
        //压缩脚本文件
        .pipe(uglify())
        //给文件添加.min后缀
        .pipe(rename({ suffix: '.min' }))
        //压缩脚本文件
        .pipe(uglify())
        //输出压缩文件到指定目录
        .pipe(gulp.dest('dist/js'))
    //提醒任务完成
    .pipe(notify({ message: 'Scripts task complete' }))
    console.log('gulp task is done'); //自定义提醒信息
});

//clean 清除文件 在任务执行前，最好先清除之前生成的文件
gulp.task('clean', function(cb) {
    return del(['dist/css', 'dist/js', 'dist/img'], cb)
});

//监听 Watch
gulp.task('testWatch', function() {
    // Watch .less files
    gulp.watch('src/**/*.less', ['testLess']); //当所有less文件发生改变时，调用testLess任务
    // Watch image files
    gulp.watch('src/**/*.img', ['testImagemin']);
    // Watch .js files
    gulp.watch('js/*.js', ['scripts']);
    // Create LiveReload server
   livereload.listen();
   // Watch any files in assets/, reload on change
   gulp.watch(['dist/**']).on('change', livereload.changed);
});

```
## 运行
可以运行单独的任务，例如

``` bash
gulp default
gulp watch
gulp clean
```
也可以运行整个任务
``` bash
gulp
```

## 总结
1.安装Node
2.安装gulp
3.新建gulpfile.js文件
4.运行


参考文献：
1.[前端自动化的目的](http://wjkang.github.io/2016/05/02/Gulp/#前端自动化的目的)
2.[gulp API](https://github.com/gulpjs/gulp/blob/master/docs/API.md)
3.[gulp教程之gulp-minify-css](http://www.ydcss.com/archives/41)
4.[gulp-notify处理报错](http://www.tuicool.com/articles/2qeuAbi)
5.[gulp教程之gulp-imagemin](http://www.ydcss.com/archives/26)
6.[使用BrowserSync浏览及相关配置](http://www.tuicool.com/articles/fUjMRn)
7.[gulp入门教程]()
