---
title: "翻旧博客：IE8的Developer Tools只是个玩具"
description: "This is a refresh of my old blog post in 2008."
date: 2023-01-14T16:30:00+08:00
lastmod: 2023-01-14
tags: [dev, web2, seo]
featured_image: "/posts/2023/refresh-ie8-dev-tool-xutour/shiny-cnblogs-ie-xutour.png"
categories: Dev
comment: true
---

## 翻旧博客

最近在想着迁移之前博客的文章过来，发现了这样一篇文章。

这是一篇发布于2008年的文章，当时正在做前端开发和SEO的一些事情，旅行社网站在当时是很早采用Web标准（Web 2.0）的，一直记得很多页面的PageRank都在5以上（*谁还记得Google的浏览器工具栏？*），我也一直想找一些记录，刚好这篇文章里有一些截图。也可以纪念一下曾经的作品，虽然现在看起来很“~~幼稚~~”，但也是当年能在搜索引擎第一页出现的非推广内容哦。

这篇博客发布于博客园，还能看到我自己定义的主题。下面是一个截图。

![shiny-cnblogs-ie-xutour](/posts/2023/refresh-ie8-dev-tool-xutour/shiny-cnblogs-ie-xutour.png)

以下是原文：

---

## [IE8的Developer Tools只是个玩具](https://www.cnblogs.com/shinyzhu/archive/2008/03/07/1095473.html)

今天在XP SP2系统上装了IE8 Beta 1，这篇日志的主题是关于IE8在Web标准方面的一点体会和一点看法。

作为一个Developer和曾经的Designer，在听说了IE8默认使用真正的标准模式渲染网页之后，特别兴奋，下面就从一个用户的角度来看看IE8的表现。

先看看页面的呈现。

测试页面：<http://xutour.com/> (这个页面是还过得去的Web标准设计，而且能通过W3C的[HTML验证](http://validator.w3.org/check?verbose=1&uri=http%3A%2F%2Fxutour.com%2F)和[CSS2验证](http://jigsaw.w3.org/css-validator/validator?profile=css21&warning=0&uri=http%3A%2F%2Fxutour.com%2F)。)

测试浏览器：IE8 beta 1 for XP 和 Firefox 2.0.0.12

图1。IE8下的页面（~~点开看大图~~）

![img](/posts/2023/refresh-ie8-dev-tool-xutour/iexutour.jpg)

图2。FF下的页面

![img](/posts/2023/refresh-ie8-dev-tool-xutour/ffxutour.jpg)

接下来，从左到右，从上到下来分析这个页面。

首先是Logo。任何web站点几乎都需要并且确实有logo，而且logo一般是链接到首页的。这个站点不例外，例外的是，它使用了CSS [Image Replacement](http://css-discuss.incutio.com/?page=ImageReplacement)的方式让您看上去是图片的logo实际上代码里只是一个文本链接，这样的目的只是让搜索引擎容易辨认。

但是很明显，在IE8下这个链接不起作用了，但logo确实看得到。其他的诸如FF，和IE7,IE6还有IE5.5都能正常显示。

看看HTML代码：

```html
<div id="logo"><a href="http://www.xutour.com/" title="旭途旅游网">旭途旅游</a></div>
```

然后看看CSS代码：

```css
#logo{float:left;width:200px;font-size:0;line-height:0;}

#logo a{display:block;width:200px;height:70px;margin:5px;font-size:0;color:#fff;line-height:0;background:url(../i/xutourlogo.gif) no-repeat top left;}
```

问题在`font-size:0;`和两个`line-height:0;`。待细查之后补上。

再看看列表样式吧。多么明显IE8的列表项前的点足足是FF下的4倍大！不知道Vista下是不是这样。

其他地方倒没太大差别，页面下面的部分有被撑大的，也是因为列表。

测试页面的效果就说到这里。再来看看主角 Developer Tools。

![img](/posts/2023/refresh-ie8-dev-tool-xutour/devtools.jpg)

我想说的是，这个玩具一点都不好玩。看看人家Firebug：

![img](/posts/2023/refresh-ie8-dev-tool-xutour/firebug.jpg)

现在来补充一下我在ff里装的add-ons：最上面的是Web Developer，下面状态栏依次有color picker，HTML Validator和Firebug。

接触和使用Web标准多年，看到全部大写的标签和没有引号的属性，让我感觉回到了HTML4时代，可现在不是啊，HTML5都出来了。罗列让我不爽的地方吧：

1，标签大写；

2，属性没有引号；

3，head里的标签不是原本的顺序（为什么？）……

看CSS标签，这个对于IE8的Developer Tools没什么好说的，仅仅简单把css的定义列了出来，而Firebug可以预览：

![img](/posts/2023/refresh-ie8-dev-tool-xutour/fbcss.jpg)

继续罗列不爽的地方：

4，CSS辅助工具非常简陋；

再来看脚本调试，在我使用的几个小时中，用IE8打开cnblogs后台编辑日志，结果不能显示编辑器，而且卡住。

js调试页面我们访问http://www.xutour.com/help/AboutXuTour.htm，因为这里面有一个添加到收藏夹的js代码。

![img](/posts/2023/refresh-ie8-dev-tool-xutour/iejsdebug1.jpg)

IE中难道只能调试内联js代码？而且调试时还提示禁用，启用之后呢？

![img](/posts/2023/refresh-ie8-dev-tool-xutour/iejsdebug11.jpg)

看来可以调试，兴冲冲地点了“添加到收藏夹”，就仅仅这个样子，Locals变量里什么都没有。

![img](/posts/2023/refresh-ie8-dev-tool-xutour/fbjsdebug.jpg)

Firebug里的调试明显详细许多许多。

OK。这玩具就玩到这里，希望下一版本有更多改进。

[IE8 is better but still slightly broken](http://archivist.incutio.com/viewlist/css-discuss/97172)