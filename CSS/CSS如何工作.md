我们已经学习了CSS的基础知识，它的用途以及如何编写简单的样式表。在本课程中，我们将研究浏览器如何使用CSS和HTML并将其转换为网页。

| 先决条件： | 基本的计算机知识，[已安装的基本软件](https://developer.mozilla.org/en-US/Learn/Getting_started_with_the_web/Installing_basic_software)，[使用文件的](https://developer.mozilla.org/en-US/Learn/Getting_started_with_the_web/Dealing_with_files)基本知识以及HTML基础（研究[HTML简介](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML)）。 |
| :--------- | ------------------------------------------------------------ |
| 目的：     | 要了解浏览器如何解析CSS和HTML的基础知识，以及当浏览器遇到它不理解的CSS时会发生什么。 |

## CSS实际如何工作？

浏览器显示文档时，必须将文档的内容与其样式信息结合在一起。它分多个阶段处理文档，我们在下面列出了这些阶段。请记住，这是浏览器加载网页时的非常简化的版本，并且不同的浏览器将以不同的方式处理该过程。但这大致是会发生的。

1. 浏览器加载HTML（例如，从网络接收HTML）。
2. 它将[HTML](https://developer.mozilla.org/en-US/docs/Glossary/HTML)转换为[DOM](https://developer.mozilla.org/en-US/docs/Glossary/DOM)（*文档对象模型*）。DOM表示计算机内存中的文档。下一部分将更详细地说明DOM。
3. 然后，浏览器将获取HTML文档链接到的大多数资源，例如嵌入式图像和视频……以及链接的CSS！在此过程中稍后会处理JavaScript，为了简化起见，在此不再讨论。
4. 浏览器解析提取的CSS，并根据选择器类型将不同的规则分类为不同的“存储桶”，例如元素，类，ID等。根据找到的选择器，它确定应将哪些规则应用于DOM中的哪些节点，并根据需要向其附加样式（此中间步骤称为渲染树）。
5. 在将规则应用到渲染树之后，渲染树将放置在该结构中。
6. 页面的视觉显示在屏幕上显示（此阶段称为绘画）。

下图还提供了该过程的简单视图。

![img](https://mdn.mozillademos.org/files/11781/rendering.svg)

## 关于DOM

DOM具有树状结构。标记语言中的每个元素，属性和一段文本都成为树结构中的[DOM节点](https://developer.mozilla.org/en-US/docs/Glossary/Node/DOM)。节点由它们与其他DOM节点的关系定义。一些元素是子节点的父节点，并且子节点具有同级。

了解DOM可以帮助您设计，调试和维护CSS，因为DOM是CSS与文档内容结合的地方。当您开始使用浏览器DevTools时，将在选择项目时浏览DOM，以查看适用的规则。

## 真实的DOM表示

让我们看一个示例，看看真实的HTML代码段如何转换为DOM，而不是冗长而乏味的解释。

采取以下HTML代码：

```html
<p>
  Let's use:
  <span>Cascading</span>
  <span>Style</span>
  <span>Sheets</span>
</p>
```

在DOM中，与我们的``元素对应的节点是父节点。它的子节点是一个文本节点，三个节点对应于我们的``元素。该`SPAN`节点也是 家长，与他们的孩子文本节点：

```html
P
├─ "Let's use:"
├─ SPAN
|  └─ "Cascading"
├─ SPAN
|  └─ "Style"
└─ SPAN
   └─ "Sheets"
```

这是浏览器解释以前的HTML代码段的方式—呈现上面的DOM树，然后在浏览器中将其输出，如下所示：



## 将CSS应用于DOM

假设我们在文档中添加了一些CSS，以对其进行样式设置。同样，HTML如下：

```html
<p>
  Let's use:
  <span>Cascading</span>
  <span>Style</span>
  <span>Sheets</span>
</p>
```

假设我们对其应用以下CSS：

```css
span {
  border: 1px solid black;
  background-color: lime;
}
```

浏览器将解析HTML并从中创建DOM，然后解析CSS。由于CSS中唯一可用的规则具有`span`选择器，因此浏览器将能够非常快速地对CSS进行排序！它将对该规则应用三个规则``，然后将最终的视觉表示绘制到屏幕上。

更新后的输出如下：



在我们的[调试CSS](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Debugging_CSS)的下一个模块中的文章中，我们将使用浏览器DevTools调试CSS问题，并了解更多有关如何在浏览器解释CSS。

## 如果浏览器遇到无法理解的CSS，会发生什么？

[在前面的课程](https://developer.mozilla.org/en-US/docs/Learn/CSS/First_steps/What_is_CSS#Browser_support)中，我提到浏览器并非都同时实现新的CSS。另外，许多人没有使用最新版本的浏览器。鉴于CSS一直在开发，因此领先于浏览器可以识别的内容，您可能会想知道，如果浏览器遇到无法识别的CSS选择器或声明，会发生什么情况。

答案是它什么都不做，只是继续前进到CSS的下一部分！

如果浏览器正在解析您的规则，并且遇到它无法理解的属性或值，则它将忽略它并继续进行下一个声明。如果您犯了一个错误并错误地拼写了一个属性或值，或者该属性或值太新并且浏览器尚不支持它，它将执行此操作。

同样，如果浏览器遇到一个无法理解的选择器，它将忽略整个规则，然后转到下一个规则。

在下面的示例中，我使用了英式英语拼写作为颜色，这使该属性无效，因为无法识别。所以我的段落没有被染成蓝色。但是，所有其他CSS均已应用；仅无效行被忽略。

```html
<p> I want this text to be large, bold and blue.</p>
p {
  font-weight: bold;
  colour: blue; /* incorrect spelling of the color property */
  font-size: 200%;
}
```



在CodePen中打开在JSFiddle中打开



此行为非常有用。这意味着您可以使用新的CSS作为增强功能，知道如果不理解它将不会发生任何错误-浏览器是否会获得新功能。加上级联的工作方式，以及当您具有两个具有相同特异性的规则时，浏览器将使用样式表中遇到的最后一个CSS的事实，您还可以为不支持新CSS的浏览器提供替代方法。

当您要使用一个非常新的值，但并非所有地方都支持该值时，此方法特别有用。例如，某些较旧的浏览器不支持`calc()`作为值。我可能会给一个框指定一个后备宽度（以像素为单位），然后继续给出一个`calc()`值为的宽度`100% - 50px`。旧的浏览器将使用像素版本，`calc()`因为他们不理解，所以忽略了这一行。新的浏览器将使用像素解释该行，但随后使用将该行覆盖，`calc()`因为该行稍后会在级联中显示。

```css
.box {
  width: 500px;
  width: calc(100% - 50px);
}
```

在后面的课程中，我们将探讨更多支持多种浏览器的方法。