既然您已经了解了什么是CSS以及使用CSS的基础知识，该是时候深入了解该语言本身的结构了。我们已经满足了这里讨论的许多概念。如果您发现以后任何令人困惑的概念，都可以回到此概述。

| 先决条件： | 基本的计算机知识，[安装基础软件](https://developer.mozilla.org/en-US/Learn/Getting_started_with_the_web/Installing_basic_software)，基础知识[处理文件](https://developer.mozilla.org/en-US/Learn/Getting_started_with_the_web/Dealing_with_files)，HTML基础知识（研究[介绍HTML](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML)），以及一个想法[如何CSS作品](https://developer.mozilla.org/en-US/docs/Learn/CSS/Introduction_to_CSS/How_CSS_works)。 |
| :--------- | ------------------------------------------------------------ |
| 目的：     | 详细了解CSS的基本语法结构。                                  |

## 将CSS应用于HTML

我们将要看的第一件事是将CSS应用于文档的三种方法。

### 外部样式表



在[CSS入门中，](https://developer.mozilla.org/en-US/docs/Learn/CSS/First_steps/Getting_started)我们将外部样式表链接到了我们的页面。这是将CSS附加到文档的最常用和最有用的方法，因为您可以将CSS链接到多个页面，从而使您可以使用相同的样式表对它们进行样式设置。在大多数情况下，网站的不同页面看起来几乎都相同，因此您可以为基本外观使用相同的规则集。

外部样式表是将CSS编写在带有`.css`扩展名的单独文件中，并从HTML<link>元素引用它的情况：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>My CSS experiment</title>
    <link rel="stylesheet" href="styles.css">
  </head>
  <body>
    <h1>Hello World!</h1>
    <p>This is my first CSS example</p>
  </body>
</html>
```

CSS文件可能如下所示：

```css
h1 {
  color: blue;
  background-color: yellow;
  border: 1px solid black;
}

p {
  color: red;
}
```

元素的`href`属性<link>需要引用文件系统上的文件。

在上面的示例中，CSS文件与HTML文档位于同一文件夹中，但是您可以将其放置在其他位置并调整指定的路径以适合，例如：

```html
<!-- Inside a subdirectory called styles inside the current directory -->
<link rel="stylesheet" href="styles/style.css">

<!-- Inside a subdirectory called general, which is in a subdirectory called styles, inside the current directory -->
<link rel="stylesheet" href="styles/general/style.css">

<!-- Go up one directory level, then inside a subdirectory called styles -->
<link rel="stylesheet" href="../styles/style.css">
```

### 内部样式表



内部样式表是您没有外部CSS文件的地方，而是将CSS放在<style>HTML 内包含的元素内<head>。

因此，HTML如下所示：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>My CSS experiment</title>
    <style>
      h1 {
        color: blue;
        background-color: yellow;
        border: 1px solid black;
      }

      p {
        color: red;
      }
    </style>
  </head>
  <body>
    <h1>Hello World!</h1>
    <p>This is my first CSS example</p>
  </body>
</html>
```

在某些情况下（这可能是有用的（也许您正在使用无法直接修改CSS文件的内容管理系统）），但是它的效率不如外部样式表–在网站中，CSS需要可以在每个页面上重复，并在需要更改的地方在多个位置进行更新。

### 内联样式



内联样式是CSS声明，仅影响一个元素，包含在`style`属性中：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>My CSS experiment</title>
  </head>
  <body>
    <h1 style="color: blue;background-color: yellow;border: 1px solid black;">Hello World!</h1>
    <p style="color:red;">This is my first CSS example</p>
  </body>
</html>
```

**除非确实需要，否则请不要这样做！**这确实对维护不利（您可能必须每个文档多次更新相同的信息），并且还会将您的演示CSS信息与HTML结构信息混合在一起，使代码更难以阅读和理解。将不同类型的代码分隔开可以使所有从事该代码工作的人都轻松得多。

在某些地方，内联样式更常见，甚至更可取。如果您的工作环境确实很严格（也许CMS仅允许您编辑HTML正文），则可能不得不使用它们。您还将看到它们在HTML电子邮件中使用了很多时间，以便与尽可能多的电子邮件客户端兼容。

## 在本文中使用CSS

本文中有很多CSS可供使用。为此，建议您在计算机上创建一个新的目录/文件夹，并在其中创建以下两个文件的副本：

**index.html：**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>My CSS experiments</title>
    <link rel="stylesheet" href="styles.css">
  </head>
  <body> 

    <p>Create your test HTML here</p>

  </body>
</html>
```

**styles.css：**

```css
/* Create your test CSS here */

p {
  color: red;
}
```

然后，当您遇到要尝试的某些CSS时，将HTML<body>内容替换为要样式的HTML，然后开始添加CSS来在CSS文件中对其进行样式设置。

继续阅读，尽享乐趣！

## 选择器

没有开会选择器就无法谈论CSS，我们已经在[CSS入门](https://developer.mozilla.org/en-US/docs/Learn/CSS/First_steps/Getting_started)教程中发现了几种不同的类型。选择器是我们如何在HTML文档中定位某些对象以便对其应用样式。如果您的样式没有应用，则您的选择器可能与您认为应该匹配的内容不匹配。

每个CSS规则都以一个选择器或一个选择器列表开头，以告知浏览器规则应应用于哪个或哪些元素。以下所有都是有效选择器或选择器列表的示例。

```css
h1
a:link
.manythings
#onething
*
.box p
.box p:first-child
h1, h2, .intro
```

**尝试创建一些使用上述选择器的CSS规则，以及一些由它们选择样式的HTML。如果您不知道上述语法的含义，请尝试在MDN上搜索！**

**注意**：在下一个模块的[CSS选择器](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Selectors)教程中，您将学到更多关于选择[器的知识](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Selectors)。

### 特异性



经常会有两个选择器可以选择同一HTML元素的情况。考虑下面的样式表，其中有一条规则，该规则的`p`选择器会将段落设置为蓝色，并且将类设置为红色。

```css
.special {
  color: red;
}

p {
  color: blue;
}
```

假设在HTML文档中，我们有一个带有类别的段落`special`。这两个规则都可能适用，那么哪一个获胜？您认为我们的段落将变成哪种颜色？

```html
<p class="special">What color am I?</p>
```

CSS语言具有规则来控制在发生冲突时哪个规则将获胜-这些规则称为**层叠**和**特异性**。在下面的代码块中，我们为`p`选择器定义了两个规则，但是该段最终显示为蓝色。这是因为将其设置为蓝色的声明出现在样式表的后面，而以后的样式将覆盖以前的样式。这是级联动作。

```css
p {
  color: red;
}

p {
  color: blue;
}
```

但是，在我们较早的带有类选择器和元素选择器的块的情况下，该类将获胜，使段落变为红色-甚至认为它出现在样式表的前面。类被描述为比元素选择器更具体或更具有特定性，因此它会获胜。

**自己尝试上述实验-将HTML添加到实验中，然后将两个`p { ... }`规则添加到样式表中。接下来，更改第一个`p`选择器以`.special`查看其如何更改样式。**

专门性规则和级联规则乍一看似乎有些复杂，一旦您掌握了更多的CSS知识，就更容易理解。在我们的下一个[级联和继承](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance)文章中，您将在下一个模块中找到该文章，我将对此进行详细说明，包括如何计算特异性。现在，请记住这已经存在，并且有时CSS可能不适用于您期望的样式，因为样式表中的其他内容具有更高的特异性。识别一个以上的规则可能适用于某个元素是解决此类问题的第一步。

## 属性和值

在最基本的层次上，CSS包含两个构建块：

- **属性**：指示文体特征（如该人可读的标识符`font-size`，`width`，`background-color`）你想改变。
- **值**：每个指定的属性都有一个值，该值指示您如何更改这些样式（例如，要将字体，宽度或背景颜色更改为哪些）。

下图突出显示了单个属性和值。属性名称为`color`，值为`blue`。

![CSS中突出显示的声明](https://mdn.mozillademos.org/files/16498/declaration.png)

与值配对的属性称为*CSS声明*。CSS声明放在*CSS声明块中*。下一张图像显示了我们的CSS，突出显示了声明块。

![突出显示的声明块](https://mdn.mozillademos.org/files/16499/declaration-block.png)

最后，CSS声明块与*选择器*配对以产生*CSS规则集*（或*CSS Rules*）。我们的图像包含两个规则，一个用于`h1`选择器，一个用于`p`选择器。的规则`h1`突出显示。

![h1的规则突出显示](https://mdn.mozillademos.org/files/16500/rules.png)

将CSS属性设置为特定值是CSS语言的核心功能。CSS引擎会计算哪些声明适用于页面的每个元素，以便对其进行适当的布局和样式设置。要记住的重要一点是，属性和值在CSS中均区分大小写。每对中的属性和值都用冒号（`:`）分隔。

**尝试查找以下属性的不同值，并编写将它们应用于不同HTML元素的CSS规则：**

- **[`font-size`](https://developer.mozilla.org/en-US/docs/Web/CSS/font-size)**
- **[`width`](https://developer.mozilla.org/en-US/docs/Web/CSS/width)**
- **[`background-color`](https://developer.mozilla.org/en-US/docs/Web/CSS/background-color)**
- **[`color`](https://developer.mozilla.org/en-US/docs/Web/CSS/color)**
- **[`border`](https://developer.mozilla.org/en-US/docs/Web/CSS/border)**

**重要说明**：如果属性未知，或者对于给定属性值无效，则声明被视为*无效，*并且浏览器的CSS引擎将完全忽略该声明。

**重要说明**：在CSS（和其他网络标准）中，美国拼写已被同意作为在出现语言不确定性的地方使用的标准。例如，`color`应*始终*拼写为`color`。`colour`将无法正常工作。

### 功能



尽管大多数值是相对简单的关键字或数字值，但有些可能的值采用函数的形式。`calc()`函数就是一个例子。此函数使您可以从CSS内进行简单的数学运算，例如：

```html
<div class="outer"><div class="box">The inner box is 90% - 30px.</div></div>
.outer {
  border: 5px solid black;
}

.box {
  padding: 10px;
  width: calc(90% - 30px);
  background-color: rebeccapurple;
  color: white;
}
```



函数由函数名称和随后的括号组成，该函数的允许值放入其中。在`calc()`上面的示例中，我要求此框的宽度为包含块宽度的90％，减去30像素。我无法提前计算出这一点，只需将值输入CSS，因为我不知道90％是多少。与所有值一样，MDN上的相关页面将包含用法示例，因此您可以了解该函数的工作方式。

另一个示例是的各种值[`transform`](https://developer.mozilla.org/en-US/docs/Web/CSS/transform)，例如`rotate()`。

```html
<div class="box"></div>
.box {
  margin: 30px;
  width: 100px;
  height: 100px;
  background-color: rebeccapurple;
  transform: rotate(0.8turn)
}
```



**尝试查找以下属性的不同值，并编写将它们应用于不同HTML元素的CSS规则：**

- **[`transform`](https://developer.mozilla.org/en-US/docs/Web/CSS/transform)**
- **[`background-image`](https://developer.mozilla.org/en-US/docs/Web/CSS/background-image)，尤其是渐变值**
- **[`color`](https://developer.mozilla.org/en-US/docs/Web/CSS/color)，尤其是rgb / rgba / hsl / hsla值**

## @规则

到目前为止，我们还没有遇到`@rules`。这些是特殊规则，可为CSS提供一些有关行为的说明。一些`@rules`简单的规则名称和值。例如，要将其他样式表导入主CSS样式表，可以使用`@import`：

```css
@import 'styles2.css';
```

`@rules`您会遇到的最常见的情况之一是`@media`，它允许您仅在满足某些条件时（例如，当屏幕分辨率超过一定数量或屏幕宽度超过一定宽度时），使用[媒体查询](https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries)来应用CSS。

在下面的CSS中，我们有一个样式表，为``元素提供了粉红色的背景色。但是，然后，我们`@media`用于创建样式表的一部分，该部分仅在视口宽度大于30em的浏览器中应用。如果浏览器的宽度大于30em，则背景颜色将为蓝色。

```css
body {
  background-color: pink;
}

@media (min-width: 30em) {
  body {
    background-color: blue;
  }
}
```

`@rules`在这些教程中，您还将遇到其他内容。

**查看是否可以将媒体查询添加到CSS中，以根据视口宽度更改样式。更改浏览器窗口的宽度以查看结果。**

## 速记

一些属性，如[`font`](https://developer.mozilla.org/en-US/docs/Web/CSS/font)，[`background`](https://developer.mozilla.org/en-US/docs/Web/CSS/background)，[`padding`](https://developer.mozilla.org/en-US/docs/Web/CSS/padding)，[`border`](https://developer.mozilla.org/en-US/docs/Web/CSS/border)，和[`margin`](https://developer.mozilla.org/en-US/docs/Web/CSS/margin)被称为**速记性能** -这是因为它们允许你在一个单行设置几个属性值，节省了时间，让您的代码更简洁的过程中。

例如，这一行：

```css
/* In 4-value shorthands like padding and margin, the values are applied
   in the order top, right, bottom, left (clockwise from the top). There are also other 
   shorthand types, for example 2-value shorthands, which set padding/margin
   for top/bottom, then left/right */
padding: 10px 15px 15px 5px;
```

一起做所有这些事情：

```css
padding-top: 10px;
padding-right: 15px;
padding-bottom: 15px;
padding-left: 5px;
```

而此行：

```css
background: red url(bg-graphic.png) 10px 10px repeat-x fixed;
```

一起做所有这些事情：

```css
background-color: red;
background-image: url(bg-graphic.png);
background-position: 10px 10px;
background-repeat: repeat-x;
background-attachment: fixed;
```

我们现在不会尝试详尽地讲这些，您将在本课程的后面看到很多示例，建议您在[CSS参考](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference)中查找速记属性名称以了解更多信息。

**尝试将上述声明添加到CSS中，以了解它如何影响HTML的样式。尝试尝试一些不同的值。**

**警告**：速记通常会允许您遗漏值，但是它们会将所有您不包括的值重置为初始值。这样可以确保使用一组合理的值。但是，如果您希望速记仅更改您传入的值，则可能会造成混淆。

## 注释

与HTML一样，鼓励您在CSS中进行注释，以帮助您理解几个月后返回的代码工作方式，并帮助其他使用该代码的人理解它。

CSS中的注释以`/*`和开头`*/`。在下面的代码块中，我已使用注释标记了不同的不同代码段的开始。当代码库变大时，这很有用，可帮助您导航-您可以在代码编辑器中搜索注释。

```css
/* Handle basic element styling */
/* -------------------------------------------------------------------------------------------- */
body {
  font: 1em/150% Helvetica, Arial, sans-serif; 
  padding: 1em; 
  margin: 0 auto; 
  max-width: 33em;
}

@media (min-width: 70em) {
  /* Let's special case the global font size. On large screen or window,
     we increase the font size for better readability */
  body {
    font-size: 130%;
  }
}

h1 {font-size: 1.5em;}

/* Handle specific elements nested in the DOM  */
/* -------------------------------------------------------------------------------------------- */
div p, #id:first-line {
  background-color: red; 
  border-radius: 3px;
}

div p {
  margin: 0; 
  padding: 1em;
}

div p + p {
  padding-top: 0;
}
```

注释对于临时*注释掉*代码的某些部分以进行测试也很有用，例如，如果您试图查找代码的哪一部分会导致错误。在下一个示例中，我注释掉了`.special`选择器的规则。

```css
/*.special { 
  color: red; 
}*/

p { 
  color: blue; 
}
```

**在CSS中添加一些注释，以习惯使用它们。**

## 空格

空白表示实际空间，制表符和换行符。与HTML一样，浏览器倾向于忽略CSS内部的大部分空白；大量的空白只是为了提高可读性。

在下面的第一个示例中，每个声明（以及规则的开始/结束）都位于其自己的行上–可以说，这是编写CSS的一种好方法，因为它易于维护和理解：

```css
body {
  font: 1em/150% Helvetica, Arial, sans-serif;
  padding: 1em;
  margin: 0 auto;
  max-width: 33em;
}

@media (min-width: 70em) {
  body {
    font-size: 130%;
  }
}

h1 {
  font-size: 1.5em;
}

div p,
#id:first-line {
  background-color: red;
  border-radius: 3px;
}

div p {
  margin: 0;
  padding: 1em;
}

div p + p {
  padding-top: 0;
}
```

您可以像这样编写完全相同的CSS，并删除了大部分空白-这在功能上与第一个示例相同，但是我敢肯定，您会同意它有些难于阅读：

```css
body {font: 1em/150% Helvetica, Arial, sans-serif; padding: 1em; margin: 0 auto; max-width: 33em;}
@media (min-width: 70em) { body {font-size: 130%;} }

h1 {font-size: 1.5em;}

div p, #id:first-line {background-color: red; border-radius: 3px;}
div p {margin: 0; padding: 1em;}
div p + p {padding-top: 0;}
```

您选择的代码布局通常是个人喜好，尽管当您开始在团队中工作时，您可能会发现现有团队有自己的样式指南，该样式指南指定了要遵循的约定。

属性及其值之间的空格，在CSS中需要注意。

例如，以下声明是有效的CSS：

```css
margin: 0 auto;
padding-left: 10px;
```

但是以下无效：

```css
margin: 0auto;
padding- left: 10px;
```

`0auto`不能识别为该`margin`属性的有效值（`0`并且`auto`是两个单独的值），并且浏览器也不能识别`padding-`为有效属性。因此，您应始终确保将不同的值彼此之间至少隔开一个空格，但应将属性名称和属性值作为单个连续的字符串保留在一起。