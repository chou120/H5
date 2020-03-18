在页面加载完成的时候，标签[head]( 1/docs/Glossary/Head)里的内容，是不会在页面中显示出来的。它包含了像页面的`<title>`(标题) ,[CSS]( 1/docs/Glossary/CSS)(如果你选择用 CSS 来为 HTML 内容添加样式)，指向自定义图标的链接和其他的元数据(描述HTML的数据，比如，作者，和描述文档的重要关键词)。本文将涵盖上述内容并拓展，为您对标记的使用打下一个良好的基础。

| 先决条件: | 熟悉HTML,可以去 [Getting started with HTML]( 1/docs/Learn/HTML/Introduction_to_HTML/Getting_started)了解 |
| :-------- | ------------------------------------------------------------ |
| 目的:     | 学习head标签，它的目的是什么，包含哪些元素以及它对HTML文档有什么影响 |

## 什么是HTML 头部?

让我们简单的回顾下上一章提到的HTML

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>My test page</title>
  </head>
  <body>
    <p>This is my page</p>
  </body>
</html>
```

HTML 头部是包含在 `<head>`元素里面的内容。不像`<body>`元素的内容会显示在浏览器中，head 里面的内容不会在浏览器中显示，它的作用是包含一些页面的[元数据]( 1/docs/Glossary/Metadata)。在下面的例子中，head 的内容很少。

```html
<head>
  <meta charset="utf-8">
  <title>My test page</title>
</head>
```

当然，在大型的页面中，head 会包含很多的元数据。你可以用 [developer tools](1/en-US/docs/Learn/Discover_browser_developer_tools) 去查看你喜欢的网页的 head 的内容。在这里，我们不打算将所有能够包含在 head 里的内容都告诉你，而是会告诉你如何使用你将要包含在 head 里的主要元素，让你对其多一些熟悉。让我们开始吧。

## 增加一个标题

我们之前已经了解过`<title>`，它可以用来给 html 文档添加一个标题。你可能会将它和给 body 添加标题的 `<h1>`元素混淆，有些时候 h1 也会被称作网页标题。但是它们是不同的。

- 当被加载到浏览器中的时候，元素 `<h1>`会出现在页面中 —— 通常它应该在一个页面中只被使用一次，它被用来标记你的页面内容的标题（故事的标题，新闻标题或者任何适当的方式）。
- 元素`<title>`是用来表示整个HTML文档标题的元数据（不是文档的内容）。

### 交互式学习：检查一个简单的例子



1. 为了开始这个交互式学习，我们希望你到我们的Github库中下载一份我们的

   [github地址](https://github.com/mdn/learning-area/blob/master/html/introduction-to-html/the-html-head/title-example.html) 。要做到这一点，你可以选择下面两种操作之一

   1. 使用你的代码编辑器，从页面中拷贝粘贴代码到一个新的文本文件中，然后将其保存到一个适当的地方。
2. 按下页面中的 "Raw" 按钮， 从浏览器的菜单中选择 *File > Save Page As...* , 然后选择一个地方来保存这个文件。
   
2. 在浏览器中打开文件，你会看到类似这样效果：

   ![A simple web page with the title set to <title> element, and the <h1> set to <h1> element.](https://mdn.mozillademos.org/files/12323/title-example.png)现在很明显的可以看到 `<h1>`出现的地方，和 `<title>`出现的地方！

3. 你应该尝试着在你的代码编辑器中打开这些代码，编辑这些元素的内容，然后在你的浏览器中刷新页面。祝你玩得开心。

元素 `<title>` 也被以其他的方式使用着。 比如说，如果你尝试为某个页面添加书签，*（Bookmarks > Bookmark This Page*， 在火狐浏览器中），你会看到 `<title> `的内容被作为建议的书签名。

![A webpage being bookmarked in firefox; the bookmark name has been automatically filled in with the contents of the <title> element ](https://mdn.mozillademos.org/files/12337/bookmark-example.png)

元素 `<title> `的内容也被用在搜索的结果中，正如你即将在下面看到的。

## 元数据：`<meta>`元素

元数据就是描述数据的数据，而HTML有一个“官方的”方式来为一个文档添加元数据—— `<meta>`元素。当然，其他在这篇文章中提到的东西也可以被当作元数据。有很多不同种类的`<meta>`元素可以被包含进你的页面的`<head>`元素，但是现在我们还不会尝试去解释所有类型，这只会引起混乱。我们会解释一些你常会看到的类型，先让你有个概念。

### 指定你的文档中字符的编码



在上面的例子中，这行是被包含的：

```html
<meta charset="utf-8">
```

这个元素简单的指定了文档的字符编码 —— 在这个文档中被允许使用的字符集。 `utf-8` 是一个通用的字符集，它包含了任何人类语言中的大部分的字符。 这意味着你的web页面可以显示任意的语言；所以对于你的每一个页面都使用这个设置会是一个好主意！比如说，你的页面可以很好的处理英语和日语：

![a web page containing English and Japanese characters, with the character encoding set to universal, or utf-8. Both languages display fine,](https://mdn.mozillademos.org/files/12343/correct-encoding.png)比如说，如果你将你的字符集设置为 `ISO-8859-1` （拉丁字母的字符集），那么你的页面会是乱码的：

![a web page containing English and Japanese characters, with the character encoding set to latin. The Japanese characters don't display correctly](https://mdn.mozillademos.org/files/12341/bad-encoding.png)

**注**: 一些浏览器（比如Chrome）会自动修正错误的编码，所以取决于你所使用的浏览器，你或许不会看到这个问题。无论如何，你仍然应该为你的页面手动设置编码为`utf-8`，来避免在其他浏览器中可能出现的潜在问题。

### 交互式学习： 体验字符



为了进行这个练习，回到你在前面`<title>`章节中获取的HTML模板 （[title-example.html page](https://github.com/mdn/learning-area/blob/master/html/introduction-to-html/the-html-head/title-example.html)），试着改变其字符集的值为`ISO-8859-1`，然后将日语添加到页面中。这就是我们使用的代码：

```html
<p>Japanese example: ご飯が熱い。</p>
```

### 添加作者和描述



许多`<meta>`元素包含了name和 content 特性：

- `name` 指定了meta 元素的类型； 说明该元素包含了什么类型的信息。
- `content` 指定了实际的元数据内容。

这两个meta 元素对于定义你的页面的作者和提供页面的简要描述是很有用的 。让我们看一个例子：

```html
<meta name="author" content="Chris Mills">
<meta name="description" content="The MDN Learning Area aims to provide
complete beginners to the Web with all they need to know to get
started with developing web sites and applications.">
```

指定作者在某些情况下是很有用的：如果你需要联系页面的作者，问一些关于页面内容的问题。 一些内容管理系统能够自动获取页面作者的信息，然后用于某些用途。

指定包含关于页面内容的关键字的页面内容的描述是很有用的，因为它可能或让你的页面在搜索引擎的相关的搜索出现得更多 （这些行为术语上被称为 [Search Engine Optimization]( 1/docs/Glossary/SEO), or [SEO]( 1/docs/Glossary/SEO).）

## 在你的站点增加自定义图标

为了进一步丰富你的网站设计，你可以在元数据中添加对自定义图标的引用，这些将在特定的场合中显示。

这个不起眼的图标已经存在很多很多年了，16 x 16 像素是这种图标的第一种类型。你可以看见这些图标出现在浏览器每一个打开的页面中的标签页中中以及在书签面板中的书签页面中。

页面添加图标的方式有：

1. 将其保存在与网站的索引页面相同的目录中，以.ico格式保存（大多数浏览器将支持更通用的格式，如.gif或.png，但使用ICO格式将确保它能在如Internet Explorer 6一样久远的浏览器显示）

2. 将以下行添加到HTML` <head>`中以引用它：

   ```html
   <link rel="shortcut icon" href="favicon.ico" type="image/x-icon">
   ```

现代浏览器在各种场合使用favicons，如打开的页面标签页和书签面板中的书签页面。下面是一个favicon 出现在书签面板中的例子：![The Firefox bookmarks panel, showing a bookmarked example with a favicon displayed next to it.](https://mdn.mozillademos.org/files/12351/bookmark-favicon.png)

如今还有很多其他的图标类型可以考虑。 例如，你可以在 MDN 主页的源代码中找到它：

```html
<!-- third-generation iPad with high-resolution Retina display: -->
<link rel="apple-touch-icon-precomposed" sizes="144x144" href="https://developer.cdn.mozilla.net/static/img/favicon144.a6e4162070f4.png">
<!-- iPhone with high-resolution Retina display: -->
<link rel="apple-touch-icon-precomposed" sizes="114x114" href="https://developer.cdn.mozilla.net/static/img/favicon114.0e9fabd44f85.png">
<!-- first- and second-generation iPad: -->
<link rel="apple-touch-icon-precomposed" sizes="72x72" href="https://developer.cdn.mozilla.net/static/img/favicon72.8ff9d87c82a0.png">
<!-- non-Retina iPhone, iPod Touch, and Android 2.1+ devices: -->
<link rel="apple-touch-icon-precomposed" href="https://developer.cdn.mozilla.net/static/img/favicon57.a2490b9a2d76.png">
<!-- basic favicon -->
<link rel="shortcut icon" href="https://developer.cdn.mozilla.net/static/img/favicon32.e02854fdcf73.png">
```

这些注释解释了每个图标的用途 - 这些元素涵盖的东西提供一个高分辨率图标，这些高分辨率图标当网站保存到iPad的主屏幕时使用。

不用担心现在实现所有这些类型的图标 - 这是一个相当先进的功能，你将不会被要求在这个课堂上学习这个知识点。 这里的主要目的是让你提前了解有这一样东西以防当你浏览其他网站的源代码时不理解源代码的含义。



**注**：如果你的网站使用了内容安全策略（Content Security Policy, CSP）来增加安全性，这个策略会应用在图标上。如果你遇到了图标没有被加载的问题，你需要确认认 [`Content-Security-Policy`]( 1/docs/Web/HTTP/Headers/Content-Security-Policy) header的 [`img-src` directive](1/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/img-src) 没有禁止访问图标。

## 在HTML中应用CSS和JavaScript

如今，几乎你使用的所有网站都会使用 [CSS]( 1/docs/Glossary/CSS) 让网页更加炫酷，使用[JavaScript]( 1/docs/Glossary/JavaScript)让网页有交互功能，比如视频播放器，地图，游戏以及更多功能。这些应用在网页中很常见，它们分别使用 `<link>`元素以及`<script>`元素。

-  `<link>`元素经常位于文档的头部。这个link元素有2个属性，rel="stylesheet"表明这是文档的样式表，而 href包含了样式表文件的路径：

  ```html
  <link rel="stylesheet" href="my-css-file.css">
  ```

- <script> 部分没必要非要放在文档头部；实际上，把它放在文档的尾部（在 </body>标签之前）是一个更好的选择，这样可以确保在加载脚本之前浏览器已经解析了HTML内容（如果脚本加载某个不存在的元素，浏览器会报错）。

### 实践操作: 在网页中应用CSS和JavaScript



1. 开始操作之前[script.js]()和 [style.css](https://github.com/mdn/learning-area/blob/master/html/introduction-to-html/the-html-head/style.css) 文件，并把它们保存到本地电脑的同一目录下，确保使用了正确的文件名和文件格式。
2. 使用浏览器和文字编辑器同时打开你的HTML文件。
3. 根据上面的信息，添加`<link>`和`<script>`部分到您的HTML文件中, 这样你的HTML就可以应用CSS和JavaScript了。

如果按照上述步骤正确地执行, 当你保存HTML文件并重新刷新浏览器的话，你会发现页面已经变样了：

![Example showing a page with CSS and JavaScript applied to it. The CSS has made the page go green, whereas the JavaScript has added a dynamic list to the page.](https://mdn.mozillademos.org/files/12359/js-and-css.png)

- JavaScript在页面中添加了一个空列表。现在当你点击列表中的任何地方，浏览器会弹出一个对话框要求你为新列表项输入一些文本内容。当你点击OK按钮，刚刚那个新的列表项会添加到页面上，当你点击那些已有的列表项，会弹出一个对话框允许你修改列表项的文本。
- CSS使页面背景变成了绿色，文本变得大了一点。它还将JavaScript添加到页面中的一些内容进行了样式化，（带有黑色边框的红色条是CSS添加到js生成的列表中的样式。）

## 为文档设定主语言

最后，值得一提的是你可以（而且确实应该）为你的站点设定语言， 这个可以通过添加lang属性到HTML开始标签中来实现 (参考 [meta-example.html](https://github.com/mdn/learning-area/blob/master/html/introduction-to-html/the-html-head/meta-example.html))，如下所示：

```html
<html lang="en-US">
```

这在很多方面都很有用。如果你的HTML文档的语言设置好了，那么你的HTML文档就会被搜索引擎更有效地索引 (例如，允许它在特定于语言的结果中正确显示)，对于那些使用屏幕阅读器的视障人士也很有用(比如， 法语和英语中都有“six”这个单词，但是发音却完全不同)。

你还可以将文档的分段设置为不同的语言。例如，我们可以把日语部分设置为日语，如下所示：

```html
<p>Japanese example: <span lang="jp">ご飯が熱い。</span>.</p>
```

这些codes是根据 [ISO 639-1](https://en.wikipedia.org/wiki/ISO_639-1) 标准定义的。