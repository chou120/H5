**[CSS](https://developer.mozilla.org/en-US/docs/Glossary/CSS)**（层叠样式表）允许您创建美观的网页，但是它如何在后台运行？本文通过一个简单的语法示例说明了CSS是什么，还介绍了有关该语言的一些关键术语。

| 先决条件： | 基本的计算机知识，[已安装的基本软件](https://developer.mozilla.org/en-US/Learn/Getting_started_with_the_web/Installing_basic_software)，[使用文件的](https://developer.mozilla.org/en-US/Learn/Getting_started_with_the_web/Dealing_with_files)基本知识以及HTML基础（研究[HTML简介](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML)）。 |
| :--------- | ------------------------------------------------------------ |
| 目的：     | 要了解CSS是什么。                                            |

在[HTML简介](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML)模块中，我们介绍了什么是HTML以及如何使用HTML标记文档。这些文档将在网络浏览器中可读。标题看起来比常规文本要大，段落会换行并在它们之间留有空格。链接带有颜色和下划线，以区别于其他文本。您所看到的是浏览器的默认样式-浏览器应用于HTML的非常基本的样式，以确保即使页面的作者未指定任何显式样式，该样式也基本可读。

![浏览器使用的默认样式](https://mdn.mozillademos.org/files/16493/html-example.png)

但是，如果所有网站看起来都是这样，那么网络将是一个无聊的地方。使用CSS，您可以精确控制HTML元素在浏览器中的外观，并使用您喜欢的任何设计显示标记。

## CSS有什么用？

正如我们之前提到的，CSS是一种语言，用于指定如何将文档呈现给用户-如何设置文档的样式，布局等。

一个**文档**通常是用标记语言结构化的文本文件- [HTML](https://developer.mozilla.org/en-US/docs/Glossary/HTML)是最常见的标记语言，但你也可能会遇到其他标记语言如[SVG](https://developer.mozilla.org/en-US/docs/Glossary/SVG)或[XML](https://developer.mozilla.org/en-US/docs/Glossary/XML)。

向用户**呈现**文档意味着将其转换为观众可以使用的形式。[浏览器](https://developer.mozilla.org/en-US/docs/Glossary/browser)，如[火狐](https://developer.mozilla.org/en-US/docs/Glossary/Mozilla_Firefox)，[铬](https://developer.mozilla.org/en-US/docs/Glossary/Google_Chrome)，或[边缘](https://developer.mozilla.org/en-US/docs/Glossary/Microsoft_Edge)，被设计成本文档视觉上，例如，在计算机屏幕上，投影仪或打印机。

**注意**：浏览器有时也称为 [用户代理](https://developer.mozilla.org/en-US/docs/Glossary/User_agent)，它基本上是表示代表计算机系统内部人员的计算机程序。浏览器是我们谈论CSS时想到的用户代理的主要类型，但是，它并不是唯一的一种。还有其他可用的用户代理，例如将HTML和CSS文档转换为PDF进行打印的用户代理。

CSS可以用于非常基本的文档文本样式-例如，更改标题和链接的[颜色](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value)和[大小](https://developer.mozilla.org/en-US/docs/Web/CSS/font-size)。它可用于创建布局，例如[将单列文本转换为](https://developer.mozilla.org/en-US/docs/Web/CSS/Layout_cookbook/Column_layouts)具有主要内容区域和侧栏[的布局](https://developer.mozilla.org/en-US/docs/Web/CSS/Layout_cookbook/Column_layouts)，以显示相关信息。它甚至可以用于[动画](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Animations)等效果。请查看本段中有关特定示例的链接。

## CSS语法

CSS是一种基于规则的语言，您可以定义规则来指定应应用于网页上特定元素或元素组的样式组。例如，“我希望页面上的主标题显示为大红色文本。”

以下代码显示了一个非常简单的CSS规则，该规则将实现上述样式：

```css
h1 {
    color: red;
    font-size: 5em;
}
```

规则以[选择器](https://developer.mozilla.org/en-US/docs/Glossary/CSS_Selector)打开。这将*选择*我们要设置样式的HTML元素。在这种情况下，我们将设置第一级标题**<h1>**的样式。

然后，我们有一组花括号`{ }`。在这些内部将是一个或多个**声明**，这些**声明**采用**属性**和**值**对的形式。每对指定我们选择的元素的属性，然后是我们想要赋予该属性的值。

在冒号之前，我们具有属性，在冒号之后，我们具有值。CSS [属性](https://developer.mozilla.org/en-US/docs/Glossary/property/CSS)具有不同的允许值，具体取决于指定的属性。在我们的示例中，我们具有`color`属性，该属性可以采用各种[颜色值](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Values_and_units#Color)。我们也`font-size`有财产。该属性可以采用各种[大小单位](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Values_and_units#Numbers_lengths_and_percentages)作为值。

CSS样式表将包含许多这样的规则，一个接一个地写。

```css
h1 {
    color: red;
    font-size: 5em;
}

p {
    color: black;
}
```

您会发现自己快速学习了一些价值观，而另一些则需要查找。MDN上的各个属性页为您提供了一种快速的方法，以在您忘记或希望知道还能用作值的其他内容时查找属性及其值。

**注意**：您可以找到MDN [CSS参考](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference)上列出的所有CSS属性页面（以及其他CSS功能）的链接。另外，每当您需要查找有关CSS功能的更多信息时，就应该习惯在自己喜欢的搜索引擎中搜索“ mdn *css-feature-name* ”。例如，尝试搜索“ mdn color”和“ mdn font-size”！

## CSS模块

由于可以使用CSS设置样式的东西太多，因此该语言分为*模块*。在探索MDN时，您会看到对这些模块的引用，并且许多文档页面都是围绕特定模块组织的。例如，您可以查看MDN对[Backgrounds and Borders](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Backgrounds_and_Borders)模块的引用，以了解其目的是什么，以及它包含哪些不同的属性和其他功能。您还将找到定义该技术的*CSS规范的*链接（请参见下文）。

在此阶段，您不必太担心CSS的结构，但是，例如，如果您知道某个属性可能会在其他类似事物中找到，并且可以因此可能在相同的规格中。 

对于特定示例，让我们回到Backgrounds and Borders模块-您可能认为这对于在此模块中定义`background-color`和`border-color`属性是合乎逻辑的。而且你会是对的。

### CSS规范



所有Web标准技术（HTML，CSS，JavaScript等）都在称为规范（或简称为“ specs”）的巨型文档中定义，这些文档由标准组织（例如[W3C](https://developer.mozilla.org/en-US/docs/Glossary/W3C)，[WHATWG](https://developer.mozilla.org/en-US/docs/Glossary/WHATWG)，[ECMA](https://developer.mozilla.org/en-US/docs/Glossary/ECMA)或Khronos）发布并进行定义这些技术应该如何表现。

CSS没什么不同-它是由W3C内的一个称为[CSS Working Group的小组开发的](https://www.w3.org/Style/CSS/)。该小组由对CSS感兴趣的浏览器供应商和其他公司的代表组成。还有其他人（被*邀请为专家*）扮演独立的声音。它们未链接到成员组织。

CSS工作组开发或指定了新的CSS功能。有时是因为特定的浏览器对具有某种功能感兴趣，而有时是因为Web设计人员和开发人员正在要求一项功能，有时是因为工作组本身已经确定了要求。CSS正在不断开发，并提供了新功能。但是，关于CSS的一个关键问题是，每个人都非常努力地工作，永远不要以破坏旧网站的方式改变事物。一个2000年建立的网站，当时使用了有限的CSS，今天仍然可以在浏览器中使用！

作为CSS的新手，您可能会发现CSS规范不胜枚举–它们是供工程师用来实现对用户代理中功能的支持，而不是供Web开发人员阅读以了解CSS。许多经验丰富的开发人员宁愿参考MDN文档或其他教程。但是，值得一提的是它们存在，了解您正在使用的CSS，浏览器支持（请参见下文）和规格之间的关系。

## 浏览器支持

一旦指定了CSS，那么只有一个或多个浏览器实现了CSS，它才对我们开发网页有用。这意味着已经编写了代码，以将CSS文件中的指令转换为可以输出到屏幕的内容。我们将在本课程[CSS的工作原理](https://developer.mozilla.org/en-US/docs/Learn/CSS/First_steps/How_CSS_works)中更多地研究这个过程。所有浏览器都无法同时实现功能是不寻常的，因此通常存在空白，您可以在某些浏览器中使用CSS的某些部分，而在其他浏览器中不能使用。因此，能够检查实施状态很有用。在MDN的每个媒体资源页面上，您都可以看到您感兴趣的媒体资源的状态，因此可以告诉您是否可以在网站上使用它。

以下是CSS `font-family`属性的compat数据图表。

[在GitHub上更新兼容性数据](https://github.com/mdn/browser-compat-data)

| 桌面                                                         |    移动    |            |                   |           |             |                        |                       |                  |                      |               |                   |             |
| :----------------------------------------------------------- | :--------: | :--------: | :---------------: | :-------: | :---------: | :--------------------: | :-------------------: | :--------------: | :------------------: | :-----------: | :---------------: | ----------- |
| 铬                                                           |    边缘    | 火狐浏览器 |     IE浏览器      |   歌剧    | 苹果浏览器  |    Android Webview     | 适用于Android的Chrome | Android版Firefox | 适用于Android的Opera | iOS上的Safari |     三星上网      |             |
| [`font-family`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family) | 全面支持1  | 全面支持12 | 全面支持1笔记打开 | 全面支持3 | 全面支持3.5 |       全面支持1        |       全面支持1       |    全面支持18    |      全面支持4       | 全面支持10.1  |     全面支持1     | 全面支持1.0 |
| `system-ui`                                                  | 全面支持56 | 全面支持79 |   不支持不打开    | 不支持不  | 全面支持43  | 全面支持9笔记 别名打开 |      全面支持56       |    全面支持56    |       不支持不       |  全面支持43   | 全面支持9别名打开 | 全面支持6.0 |

![铬](https://developer.mozilla.org/static/browsers/chrome.b49946f7739f.svg)

![边缘](https://developer.mozilla.org/static/browsers/edge.a5b352eb863f.svg)

![火狐浏览器](https://developer.mozilla.org/static/browsers/firefox.1c9f202ae696.svg)

![IE浏览器](https://developer.mozilla.org/static/browsers/internet-explorer.4bb2b5f99e82.svg)

![歌剧](https://developer.mozilla.org/static/browsers/opera.da441711d2f6.svg)

![苹果浏览器](https://developer.mozilla.org/static/browsers/safari.aca6ae03b671.svg)

![Android Webview](https://developer.mozilla.org/static/platforms/android.201ce5d041f7.svg)

![适用于Android的Chrome](https://developer.mozilla.org/static/browsers/chrome.b49946f7739f.svg)

![Android版Firefox](https://developer.mozilla.org/static/browsers/firefox.1c9f202ae696.svg)

![适用于Android的Opera](https://developer.mozilla.org/static/browsers/opera.da441711d2f6.svg)

![iOS上的Safari](https://developer.mozilla.org/static/browsers/safari.aca6ae03b671.svg)

![三星上网](https://developer.mozilla.org/static/browsers/samsung-internet.8faa2ee1b8a1.svg)

font-familysystem-ui

------

[我们缺少什么？](https://developer.mozilla.org/en-US/docs/Learn/CSS/First_steps/What_is_CSS#)

### 传说

- 全力支持 

  全力支持

- 没有支持 

  没有支持

- 请参阅实施说明。

  请参阅实施说明。

- 使用非标准名称。

  使用非标准名称。