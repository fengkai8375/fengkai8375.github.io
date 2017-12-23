---
layout: post
title: "解决微信浏览器返回上一页面强制刷新的问题"
date: 2017-01-08 09:00:00 +0800 
categories: JavaScript
tag: Html5
---
* content
{:toc}

微信浏览器，Chrome PC版浏览器，在返回上一页面，且上一页面包含AJAX代码时，页面就会被强制刷新，极度影响用户体验。

而我们想要的效果是：返回上一页面时，页面还停留在原来的状态，AJAX获取到的数据还在，滚动条也在原来的位置。

通过**HTML5**的**history API** + **缓存**可以做到这一点。

<!-- more -->

### 原理

1. 通过history API的 history.pushState或 history.replaceState 保存AJAX状态

2.  同时将获取到的数据缓存起来（我用的是localStorage）
 
3.  当返回到这个页面时，先获取窗口的history.state，如果不为空，表示保存的有状态，我们要做的就是恢复到这个状态。
 
4.  读取缓存数据并加载。如果涉及到分页，且是滚动加载的形式，需要将当前页设置为history.state中的页数。

### history API

HTML5 history API只包括2个方法：```history.pushState()```和```history.replaceState()```，以及1个事件：```window.onpopstate```。

#### history.pushState()
它的完全体是 ```history.pushState(stateObject, title, url)```，包括三个参数。

第1个参数是状态对象，它可以理解为一个拿来存储自定义数据的元素。它和同时作为参数的url会关联在一起。

第2个参数是标题，是一个字符串，目前各类浏览器都会忽略它（以后才有可能启用，用作页面标题），所以设置成什么都没关系。目前建议设置为空字符串。

第3个参数是URL地址，一般会是简单的?page=2这样的参数风格的相对路径，它会自动以当前URL为基准。需要注意的是，本参数URL需要和当前页面URL同源，否则会抛出错误。

调用```pushState()```方法将新生成一条历史记录，方便用浏览器的“后退”和“前进”来导航（“后退”可是相当常用的按钮）。另外，从URL的同源策略可以看出，```HTML5 history API```的出发点是很明确的，就是让无跳转的单站点也可以将它的各个状态保存为浏览器的多条历史记录。当通过历史记录重新加载站点时，站点可以直接加载到对应的状态。

#### history.replaceState()
它和```history.pushState()```方法基本相同，区别只有一点，```history.replaceState()```不会新生成历史记录，而是将当前历史记录替换掉。

#### window.onpopstate
push的对立就是pop，可以猜到这个事件是在浏览器取出历史记录并加载时触发的。但实际上，它的条件是比较苛刻的，几乎只有点击浏览器的“前进”、“后退”这些导航按钮，或者是由JavaScript调用的```history.back()```等导航方法，且切换前后的两条历史记录都属于同一个网页文档，才会触发本事件。

上面的“同一个网页文档”请理解为JavaScript环境的document是同一个，而不是指基础URL（去掉各类参数的）相同。也就是说，只要有重新加载发生（无论是跳转到一个新站点还是继续在本站点），JavaScript全局环境发生了变化，```popstate```事件都不会触发。

popstate事件是设计出来和前面的2个方法搭配使用的。一般只有在通过前面2个方法设置了同一站点的多条历史记录，并在其之间导航（前进或后退）时，才会触发这个事件。同时，前面2个方法所设置的状态对象（第1个参数），也会在这个时候通过事件的```event.state```返还回来。

此外请注意，```history.pushState()```及```history.replaceState()```本身调用时是**不触发popstate事件的**。pop和push毕竟不一样！

