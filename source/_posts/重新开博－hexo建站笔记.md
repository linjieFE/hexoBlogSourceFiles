---
title: 重新开博－hexo建站笔记
date: 2016-01-20 09:51:37
categories: 学习笔记
tags:
- hexo
- 随笔
---
这两天重新搭了hexo博客.过程中一直不停的踩坑踩到脚软。一年前我搭过一个hexo博客。
由于博客的域名前缀必须跟github账号名一至。介于以前的github用户名有点土，不符合我的个人风格，
于是为了更土一点就重新注了个github账号。好了,开始入坑：
<!-- more -->
坑一：
重新关联个帐呈本来不是什么难事，我以为本地仓库不用管，只要是把远程对接仓库的ssh和本地ssh对应上就好了。
事实证明我to young to simple。
本地的全局配置的用户名和邮箱没改，所以每次提交都说我的远程仓库没权限管理以前用户（yea..，土就不说全名了）。
执行查看了一下全局配置：
``` bash
$ git config -l
```
 发现全局用户名和邮箱还是以前的，把这些依次改了，提交前会提示输入sshkey的密码。问题解决
（其实这个问题上周五就解决过一次，当时不知道为啥好了。关机周一再启动又坏了。总之做之前先pull一下再说）以上是坑一。

坑二:
hexo搭建的时候我npm和hexo都全局安了。但hexo init后总报错.提示init完还要在npm install一下。
我自作聪明的以为npm都全局安了就不用再install了。结果再次证明我to yuong to simple,
老老实实按照提示安装（npm更新到最新版本以后不再提示再装）

``` bash
$ npm install 
```
出现三个报错。后来在网上找了解决方案。

``` bash
$ sudo npm install --no-optional
```

不报了错了。其实这个报错不影响什么，但每执行都跟着就是很烦,强迫症的人不能忍。
（ps:之后升级了npm以后这个错误又出现了,所以如果你也是mac能sudo尽量sudo吧）
问题解决继续下一步，
``` bash
$ hexo generate。
```
启动本地服务：
``` bash
$ hexo server
```
启动服务后可以预览。发现皮肤太丑。就去git上找了个主题。下载完按要求到cd themes/xxx 执行了↪git pull。 
发现主题没更新!觉得是缓存就hexo clean 下。我以为跟fis一样关掉再重新监听一下就好了。结果又一次证明我to yuong to simple。本地彻底访问不了了。一气之下把终端关了再启动，提示我没有npm指令。奇怪的是node和依赖npm下的各种包webpage.gulp less都在。上网求解答有人出主意说把npm下的这些包都删了，重新装npm再把这些包一个个安上就好了。我打开npm的目录一看里面不下十个。
够安一阵了。一想姐如果就为搭个hexo把电脑都重装了也真是够了。
再一琢磨，好像npm是集成在node上的。抱着试试看的心里重新安了一下node，npm好了。但博客依然访问不了仍报错。

就这个问题去找我们前端组的领袖@凯哥[凯歌博客](http://blog.css6.com)。他已经是hexo的老用户了,博客建的很好，经常去学习。
哥看完对比了下他自己配置环境与我的不同，揣测我可能是node和npm版本太高了，建议我下个稳定版的。白天大家有工作缠身也不便多扰。
大神给了建议就照着路子回去自己摸索吧。
回座位在网上找了半天低版本无果。我心想既然如此。索性就把所有都升到最高。于是
``` bash
npm upload npm -g
```
把npm等类都升到了最高。果然能跑起来了。然后我拷了模板（模板拷到themes目录在把就可以了，但是 git pull：相当于是从远程获取最新版本并merge到本地。git pull origin master相当于git fetch 和 git merge ） 和配置文件（配置文件不能拷贝覆盖？反正我只要逐行修改保存就可以，直接覆盖就崩溃，内容始终没找到差异，有知道的朋友欢迎指证）。
我对着凯哥的配置文件一行行改。改一行启动服务一次查看，从头对到尾也没找出差异。
继续下一步

``` bash
hexo new "文章名"
```
随便编了个内容部署到github:
``` bash
hexo deloy
```

报了一个错  hexo ERROR Deployer not found: github
找了解决方案hexo3.0以上的版本:
1.安装sudo npm install hexo-deployer-git --save
2.将deploy 的 type由github改为git

再次deloy成功部署到gibhub上。页面是404我的git用户名叫linjieFE。所以我的博客也得叫linjieaFE.github.io[我的博客地址](http://linjieaFE.github.io)。
但是不能大写改成jinjiefe就好了。
最后终于大功告成了。

这里只是蜻蜓点水，如果想把博客建的更好更漂亮，
一定要好好研读[hexo文档](http://wiki.jikexueyuan.com/project/hexo-document/)
这里提供一些参考文档,也希望能对见到此文的人有所帮助都少踩坑:
[Hexo 静态博客使用指南](http://www.jianshu.com/p/73779eacb494)
[hexo常见问题解决方案](http://wp.huangshiyang.com/hexo%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88)
