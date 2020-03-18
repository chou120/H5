当你给网页或者网页应用编写客户端的JavaScript时， 你很快会遇上应用程序接口（API ）—— 这些编程特性可用来操控网站所基于的浏览器与操作系统的不同方面，或是操控由其他网站或服务端传来的数据。在这个单元里，我们将一同探索什么是API，以及如何使用一些在你开发中将经常遇见的API。

## 预备知识

若想深入理解这个单元的内容, 你必须能够以自己的能力较好地学完之前的几个章节 ([JavaScript第一步]( 1/docs/Learn/JavaScript/First_steps), [JavaScript]( 1/docs/Learn/JavaScript/First_steps)[基础要件]( 1/docs/Learn/JavaScript/Building_blocks), 和[JavaScript]( 1/docs/Learn/JavaScript/First_steps)[对象介绍]( 1/docs/Learn/JavaScript/Objects)). 这几部分涉及到了许多简单的API的使用， 如果没有它们我们将很难做一些实际的事情。在这个教程中，我们会认为你懂得JavaScript的核心知识，而且我们将更深入地探索常见的网页API。

若你知道 [HTML](1/en-US/docs/Learn/HTML) 和 [CSS](1/en-US/docs/Learn/CSS) 的基本知识，也会对理解这个单元的内容大有裨益。

注意：如果你正在使用一台无法创建你自身文件的电脑、平板或其他设备，你可以尝试使用一些在线网页编辑器如[JSBin](http://jsbin.com/)或者[Thimble](https://thimble.mozilla.org/)，来尝试编辑一些代码示例。

## 向导

- [Web API简介]( 1/docs/Learn/JavaScript/Client-side_web_APIs/Introduction)

  首先, 我们将从一个更高的角度来看这些API —它们是什么，它们怎么起作用的，你该怎么在自己的代码中使用它们以及他们是怎么构成的？ 我们依旧会再来看一看这些API有哪些主要的种类和他们会有哪些用处。

- [操作文档]( 1/docs/Learn/JavaScript/Client-side_web_APIs/Manipulating_documents)

  当你在制作WEB页面和APP时,一个你最经常想要做的事就是通过一些方法来操作WEB文档。这其中最常见的方法就是使用文档对象模型Document Object Model (DOM)，它是一系列大量使用了 [`Document`]( 1/docs/Web/API/Document) object的API来控制HTML和样式信息。通过这篇文章，我们来看看使用DOM方面的一些细节， 以及其他一些有趣的API能够通过一些有趣的方式改变你的环境。

- [从服务器获取数据]( 1/docs/Learn/JavaScript/Client-side_web_APIs/Fetching_data)

  在现代网页及其APP中另外一个很常见的任务就是与服务器进行数据交互时不再刷新整个页面，这看起来微不足道，但却对一个网页的展现和交互上起到了很大的作用，在这篇文章里，我们将阐述这个概念，然后来了解实现这个功能的技术，例如 [`XMLHttpRequest`]( 1/docs/Web/API/XMLHttpRequest) 和 [Fetch API](1/en-US/docs/Web/API/Fetch_API).（抓取API）。

- [第三方 API]( 1/docs/Learn/JavaScript/Client-side_web_APIs/Third_party_APIs)

  到目前为止我们所涉及的API都是浏览器内置的，但并不代表所有。许多大网站如Google Maps, Twitter, Facebook, PayPal等，都提供他们的API给开发者们去使用他们的数据（比如在你的博客里展示你分享的推特内容）或者服务（如在你的网页里展示定制的谷歌地图或接入Facebook登录功能）。这篇文章介绍了浏览器API和第三方API 的差别以及一些最新的典型应用。

- [绘制图形]( 1/docs/Learn/JavaScript/Client-side_web_APIs/Drawing_graphics)

  浏览器包含多种强大的图形编程工具，从可缩放矢量图形语言Scalable Vector Graphics ([SVG]( 1/docs/Web/SVG)) language，到HTML绘制元素 `<canvas>`元素([The Canvas API]( 1/docs/Web/API/Canvas_API) and [WebGL]( 1/docs/Web/API/WebGL_API)). 这篇文章提供了部分canvas的简介，以及让你更深入学习的资源。

- [视频和音频 API]( 1/docs/Learn/JavaScript/Client-side_web_APIs/Video_and_audio_APIs)

  HTML5能够通过元素标签嵌入富媒体——`<video>` and `<audio>`——而将有自己的API来控制回放，搜索等功能。本文向您展示了如何创建自定义播放控制等常见的任务。

- [客户端存储](1/en-US/docs/Learn/JavaScript/Client-side_web_APIs/Client-side_storage)

  现代web浏览器拥有很多不同的技术，能够让你存储与网站相关的数据，并在需要时调用它们，能够让你长期保存数据、保存离线网站及其他实现其他功能。本文解释了这些功能的基本原理