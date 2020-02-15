---
title: react-native 第一个demo
date: 2016-10-20 15:18:06
categories: 学习笔记
tags:
- 学习笔记
- react-native
---
# React Native Mac环境搭建

## 1、安装Homebrew
安装Homebrew是为安装Node.js做前提准备。 
安装命令:
``` bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

注：可以通过下面命令查看Homebrew是否安装成功
``` bash
brew -v
Homebrew/homebrew-core (git revision 93f1; last commit 2016-05-23)
```

## 2、安装Node.js

下载双击安装即可： 
下载地址： 
[https://nodejs.org/en/](https://nodejs.org/en/)
Node.js 需要 4.0 及其以上版本。安装好之后，npm 也有了。

## 3、安装React Native 命令行工具

``` bash
sudo npm install -g react-native-cli
```

## 4、安装Xcode，估计iOS开发的基本都安装有.

Xcode 7.0 或更高版本下载地 址： 
[https://developer.apple.com/xcode/downloads/](https://developer.apple.com/xcode/downloads/)

## 5、安装 watchman 和 flow

监控文件变化和类型检查的。安装如下

``` bash 
brew install watchman
brew install flow
```

## 6、初始化一个项目

打开终端，在某个目录下输入命令(这一步会有点慢，耐心等待一下): 
``` bash
react-native init HelloWorld 
```
### 运行项目

![](img/rn-desk.png)

用XCode打开ios/HelloWorld.xcodeproj文件，点击键盘"⌘-R”或者点击"Run"，编译运行项目。会启动React-Native服务和iOS模拟器。
在iOS模拟器中可以看到如图界面：
![](img/rn_helloworld.png)

React-Native服务在编写过程中要一直开着 如果不小心把它关了，没关系，可以在终端输入：

``` bash
npm start
```
来重新开启服务

好了，看到这里，如果你已经成功配置了React-Native的环境，并且新建并成功运行了第一个程序了。那么，就先恭喜了，我们甚至没有写一行代码，就已经成功运行了第一个React-Native的程序，是不是还挺简单的。正所谓，良好的开端是成功的一半。





