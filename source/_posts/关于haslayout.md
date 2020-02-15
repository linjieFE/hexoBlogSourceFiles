---
title: '关于IE的下的haslayout问题'
date: 2016-01-26 10:58:48
tags:
  - 学习笔记
categories: 学习笔记
---
今天早上我们一个前端同事GQL遇到一个问题。在IE7下position:relative层在动态渲染时出现偏移.
跟下面这个demo情景相似

``` html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
 "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">

<html xmlns="http://www.w3.org/1999/xhtml">
  <head>
    <title>IE7 absolute positioning bug</title>
    <style type="text/css">
      #panel { position: relative; border: solid 1px black; }
      #spacer { height: 100px; }
      #footer { position: absolute; bottom: 0px; }
    </style>
    <script type="text/javascript">
      function toggle() {
        var spacer = document.getElementById("spacer");
        var style = "block";
        if (spacer.style.display == "block" || spacer.style.display == "") {
          style = "none";
        }
        spacer.style.display = style;
      }
    </script>
  </head>
  <body>
    <div id="panel">
      <button onclick="toggle();">Click me</button>
      <br /><br /><br />
      <div id="spacer"></div>
      <div id="footer">This is some footer</div>
    </div>
  </body>
</html>
```

在IE7中，点击"Click me"，“This is some footer”没有随着边框向上走。在IE8/9/10/11，firefox里面都是正常。

二、原因分析
1 \#panel没有布局，因此当#panel重新布局的时候，IE7没有重画#panel的孩子。
2 position:relative没有触发IE的hasLayout属性。只有当元素的hasLayout属性被设为了true，才能对自己和子孙元素进行尺寸计算和定位。
3 overflow:hidden和float:left和zoom:1都能触发hasLayout属性。
<!--more-->
至于为什么一定要触发hasLayout属性，才会对元素和元素的子孙进行重新定位，请参照【资料1】。

三、解决方法
\#panel的属性中添加 overflow:hidden，或者添加float:left;width:100%; 再或者zoom:1 。这三种方法都是可以修复这个bug的。

四、参考资料
资料1：[IE7 relative/absolute positioning bug with dynamically modified page content](http://stackoverflow.com/questions/2473171/ie7-relative-absolute-positioning-bug-with-dynamically-modified-page-content)

即然问题出在haslayout上，那下面就着重介绍一下haslayout

# haslayout详解

IE，这个令所有网站设计人员讨厌，但又不得不为它工作的浏览器。不论是6、7还是8，它们都有一个共同的渲染标准haslayout，所以haslayout 是一个非常有必要彻底弄清除的概念。大多 数IE下的显示错误，就是源于它。

## 什么是Layout呢？

"Layout"是IE的一个私有属性，并不是W3C标准。它决定了一个对象（就是一个标签div、li等）在内容中如何显示、与周围对象的位置关系、以及怎样响应程序或用户产生的事件。

这个属性可以被一些css强制激活。一些HTML标签默认具有haslayout。
PS：一个对象的layout属性被激活，它的具体表现就是haslayout=true。我们可以用IE Developer Toolbar工具看到被激活的对象带有"haslayout = -1"的属性。

## 下面这些标签默认拥有haslayout属性：


``` html
<html>, <body>
<table>, <tr>, <th>, <td>
<img>
<hr>
<input>, <button>, <select>, <textarea>, <fieldset>, <legend>
<iframe>, <embed>, <object>, <applet>
<marquee>
```
<!--<p>&lt;html&gt;, &lt;body&gt;
&lt;table&gt;, &lt;tr&gt;, &lt;th&gt;, &lt;td&gt;
&lt;img&gt;
&lt;hr&gt;
&lt;input&gt;, &lt;button&gt;, &lt;select&gt;, &lt;textarea&gt;, &lt;fieldset&gt;, &lt;legend&gt;
&lt;iframe&gt;, &lt;embed&gt;, &lt;object&gt;, &lt;applet&gt;
&lt;marquee&gt;</p>-->


你可能就问：微软干嘛要设layout这个东西呢？当一个对象的layout被激活时，它以及它的子对象的定位和尺寸计算将独立进行，不受附近对象 的干扰。也就是说它拥有一个独立的布局（layout）。因此浏览器要花费更多的代价来处理拥有haslayout的对象。为了提高性能，微软增加了 layout这个IE私有的概念。

## 怎样激活layout？

### 下面列出的css属性可以激活对象的layout：

position: absolute
设置绝对定位可能会引发新的问题。
float: left|right
IE下的浮动也会产生一些莫名其妙的问题。
display: inline-block
当一个内联元素需要haslayout属性时就需要用它，但是IE本身不支持inline-block的，只是表现得像标准里说的inline-block。
width: 除'auto'外的任意值
优先考虑使用该属性。
height: 除'auto'外的任意值
对 IE6 及更早版本来说很常用，该方法被称为霍莉破解(Holly hack)，即设定这个元素的高度为 1% (height:1%;)。但是要注意，当这个元素的 overflow 属性被设置为 visible 时，这个方法就失效了。
zoom: 除'normal'外的任意值
又一个ie私有属性，不兼容标准。zoom:1可以在测试或者不追求标准的情况下使用，效果不错。
writing-mode: tb-rl
ie私有属性，不推荐用。
### IE7 还有一些额外的属性：

min-height: (任意值)
max-height: (除 none 外任意值)
min-width: (任意值)
max-width: (除 none 外任意值)
overflow: (除 visible 外任意值)
overflow-x: (除 visible 外任意值)
overflow-y: (除 visible 外任意值)
position: fixed
重置haslayout

### 在没有其它属性激活layout的情况下，使用下面的css可以重置haslayout属性：

width, height (设为 "auto")
max-width, max-height (设为 "none")(在 IE 7 中)
position (设为 "static")
float (设为 "none")
overflow (设为 "visible") (在 IE 7 中)
zoom (设为 "normal")
writing-mode (从 "tb-rl" 设为 "lr-t")
display 属性的不同：当用"inline-block"激活了haslayout 属性时，就算在一条独立的规则中覆盖这个属性为"block"或"inline"，haslayout 这个标志位也不会被重置为 false。

把 mid-width, mid-height 设为它们的默认值"0"仍然会赋予 hasLayout，但是 IE 7 却可以接受一个不合法的属性"auto"来重置 hasLayout。
