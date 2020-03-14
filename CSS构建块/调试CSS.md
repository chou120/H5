有时在编写CSS时，您会遇到一个问题，即CSS似乎并没有达到您的期望。也许您认为某个选择器应该与某个元素匹配，但是什么也没有发生，或者一个盒子的大小与您预期的不同。本文将为您提供有关调试CSS问题的指南，并向您展示所有现代浏览器中包含的DevTools如何帮助您了解正在发生的情况。

| 先决条件： | 基本的计算机知识，[已安装的基本软件](https://developer.mozilla.org/en-US/Learn/Getting_started_with_the_web/Installing_basic_software)，[使用文件的](https://developer.mozilla.org/en-US/Learn/Getting_started_with_the_web/Dealing_with_files)基本知识，HTML基础（研究[HTML简介](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML)）以及CSS的工作原理（研究[CSS第一步）](https://developer.mozilla.org/en-US/docs/Learn/CSS/First_steps)。 |
| :--------- | ------------------------------------------------------------ |
| 目的：     | 了解什么是浏览器DevTools的基础知识，以及如何进行CSS的简单检查和编辑。 |

## 如何访问浏览器DevTools

“ [什么是浏览器开发人员工具”一文](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools)是最新指南，解释了如何在各种浏览器和平台中访问这些工具。尽管您可能选择主要在特定的浏览器中进行开发，因此将最熟悉该浏览器中包含的工具，但值得了解如何在其他浏览器中访问它们。如果您在多个浏览器之间看到不同的呈现，这将有所帮助。

您还将发现浏览器在创建DevTools时已选择专注于不同领域。例如，在Firefox中，有一些出色的工具可以直观地使用CSS布局，使您可以检查和编辑[Grid Layouts](https://developer.mozilla.org/en-US/docs/Tools/Page_Inspector/How_to/Examine_grid_layouts)，[Flexbox](https://developer.mozilla.org/en-US/docs/Tools/Page_Inspector/How_to/Examine_Flexbox_layouts)和[Shapes](https://developer.mozilla.org/en-US/docs/Tools/Page_Inspector/How_to/Edit_CSS_shapes)。但是，所有不同的浏览器都具有相似的基本工具，例如，用于检查应用于页面元素的属性和值，并从编辑器对其进行更改。

在本课程中，我们将介绍Firefox DevTools的一些有用的功能，这些功能可用于CSS。为此，我将使用[一个示例文件](https://mdn.github.io/css-examples/learn/inspecting/inspecting.html)。如果要继续，请在新选项卡中加载它，然后按上面链接的文章中所述打开您的DevTools。

## DOM与视图源

可以使新手跳入DevTools的地方是，您在[查看](https://developer.mozilla.org/en-US/docs/Tools/View_source)网页[源](https://developer.mozilla.org/en-US/docs/Tools/View_source)或查看放在服务器上的HTML文件时所看到的内容与在DevTools 的[HTML窗格](https://developer.mozilla.org/en-US/docs/Tools/Page_Inspector/UI_Tour#HTML_pane)中所看到的内容之间的差异。尽管它看起来与通过“查看源”可以看到的大致相似，但还是有一些区别。

在呈现的DOM中，浏览器可能已为您纠正了一些写得不好的HTML。如果您错误地关闭了一个元素，例如打开an，<h2>但用</h3>关闭，则浏览器将弄清楚您要执行的操作，而DOM中的HTML将正确地<h3>使用</h3>。浏览器还将标准化所有HTML，而DOM也将显示JavaScript所做的任何更改。

相比之下，View Source只是存储在服务器上的HTML源代码。DevTools中的[HTML树可](https://developer.mozilla.org/en-US/docs/Tools/Page_Inspector/How_to/Examine_and_edit_HTML#HTML_tree)准确显示浏览器在任何给定时间呈现的内容，因此您可以洞悉实际情况。

## 检查已应用的CSS

在页面上选择一个元素，方法是右键/按住Ctrl键单击并选择*Inspect*，或者从DevTools显示屏左侧的HTML树中选择它。尝试选择类别为的元素`box1`；这是页面上第一个带有边框的元素。

![打开DevTools的本教程示例页面。](https://mdn.mozillademos.org/files/16606/inspecting1.png)

如果[查看](https://developer.mozilla.org/en-US/docs/Tools/Page_Inspector/UI_Tour#Rules_view) HTML右侧的[“规则”视图](https://developer.mozilla.org/en-US/docs/Tools/Page_Inspector/UI_Tour#Rules_view)，则应该能够看到应用于该元素的CSS属性和值。您将看到直接应用于类的规则`box1`，以及框从其祖先（在本例中为）继承的CSS <body>。如果您看到一些意想不到的CSS应用，这将很有用。也许它是从父元素继承而来的，您需要添加一个规则以在此元素的上下文中覆盖它。

扩展速记属性的功能也很有用。在我们的示例中，`margin`使用了速记。

**单击小箭头以展开视图，显示不同的长期属性及其值。**

**当该面板处于活动状态时，您可以在“规则”视图中打开和关闭值，如果您将鼠标悬停在其上方，则会出现复选框。取消选中规则的复选框，例如`border-radius`，CSS将停止应用。**

您可以使用它来进行A / B比较，确定是否应用规则使外观看起来更好，还可以帮助调试它-例如，如果布局出错，并且您正在尝试找出引起该属性的原因问题。

## 编辑值

除了打开和关闭属性外，您还可以编辑其值。也许您想查看另一种颜色是否看起来更好，或者希望调整某个东西的大小？DevTools可以节省大量时间来编辑样式表和重新加载页面。

**随着`box1`选定后，点击色标（小型彩色圆圈），它显示应用到边框的颜色。将会打开一个颜色选择器，您可以尝试一些不同的颜色。这些将在页面上实时更新。以类似的方式，您可以更改边框的宽度或样式。**

![带有颜色选择器的DevTools样式面板。](https://mdn.mozillademos.org/files/16607/inspecting2-color-picker.png)

## 添加新属性

您可以使用DevTools添加属性。也许您已经意识到，您不想让框继承<body>元素的字体大小，而是要设置其自己的特定大小？您可以先在DevTools中进行尝试，然后再将其添加到CSS文件中。

**您可以单击规则中的右花括号以开始向其中输入新的声明，此时您可以开始键入新属性，DevTools将显示匹配属性的自动完成列表。选择之后`font-size`，输入您想要尝试的值。您也可以单击+按钮，使用相同的选择器添加其他规则，然后在其中添加新规则。**

![DevTools面板，为规则添加了一个新属性，并且自动完成字体打开](https://mdn.mozillademos.org/files/16608/inspecting3-font-size.png)

**注意**：“规则”视图中还有其他有用的功能，例如，带有无效值的声明被划掉。您可以在[检查和编辑CSS中](https://developer.mozilla.org/en-US/docs/Tools/Page_Inspector/How_to/Examine_and_edit_CSS)找到更多信息。

## 了解盒子模型

在先前的课程中，我们讨论[了Box模型](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/The_box_model)，还有一个替代[的Box模型](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/The_box_model)，该模型可以根据您给它们提供的尺寸以及填充和边框来更改元素尺寸的计算方式。DevTools确实可以帮助您了解如何计算元素的大小。

“ [布局”视图为](https://developer.mozilla.org/en-US/docs/Tools/Page_Inspector/UI_Tour#Layout_view)您显示了所选元素上的盒子模型的示意图，并描述了更改元素布局方式的属性和值。其中包括对属性的描述，这些属性可能尚未在元素上显式使用，但确实设置了初始值。

在此面板中，详细属性之一是`box-sizing`属性，该属性控制元素使用的框模型。

**将两个框与类`box1`和比较`box2`。它们都应用了相同的宽度（400px），但是`box1`在视觉上更宽。您可以在布局面板中看到它正在使用`content-box`。该值采用您给元素的大小，然后加上填充和边框宽度。**

类别为的元素`box2`正在使用`border-box`，因此此处的填充和边框是从给定元素的大小中减去的。这意味着该框在页面上占据的空间就是您指定的确切大小，在本例中为`width: 400px`。

![DevTools的Layout部分](https://mdn.mozillademos.org/files/16609/inspecting4-box-model.png)

**注意**：有关[检查和检查Box模型的更多信息](https://developer.mozilla.org/en-US/docs/Tools/Page_Inspector/How_to/Examine_and_edit_the_box_model)。

## 解决特异性问题

有时在开发期间，但是特别是当您需要在现有站点上编辑CSS时，您会发现很难应用一些CSS。无论您做什么，该元素似乎都不会采用CSS。通常，这里发生的是一个更具体的选择器覆盖了您的更改，而DevTools确实可以为您提供帮助。

在我们的示例文件中，<em>元素中包含了两个单词。一个显示为橙色，另一个显示为粉红色。在CSS中，我们应用了：

```css
em {
  color: hotpink;
  font-weight: bold;
}
```

但是，在样式表中的上方是带有`.special`选择器的规则：

```css
.special {
  color: orange;
}
```

您会从[级联和继承](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance)的课程中讨论过特异性的课程中回想起，类选择器比元素选择器更具体，因此这就是适用的值。DevTools可以帮助您发现此类问题，尤其是当信息隐藏在巨大的样式表中的某个位置时。

**使用<em>和的类进行检查`.special`，DevTools将显示橙色是适用的颜色，还显示了`color`应用于被划掉的em 的属性。现在，您可以看到该类正在覆盖元素选择器。**

![选择一个em并查看DevTools以查看覆盖颜色的内容。](https://mdn.mozillademos.org/files/16610/inspecting5-specificity.png)

## 了解有关Firefox DevTools的更多信息

MDN上有很多有关Firefox DevTools的信息。看一下主要的[DevTools部分](https://developer.mozilla.org/en-US/docs/Tools)，有关本课中我们简要介绍的内容的更多详细信息，请参见[《指南》](https://developer.mozilla.org/en-US/docs/Tools/Page_Inspector#How_to)。

## CSS中的调试问题

在解决CSS问题时，DevTools可以提供很大的帮助，因此当您发现自己的CSS表现不如预期时，应该如何解决呢？以下步骤应有所帮助。

### 从问题退后一步



任何编码问题都可能令人沮丧，尤其是CSS问题，因为您通常不会在在线搜索时获得错误消息来帮助您找到解决方案。如果您感到沮丧，请暂时离开该问题一会儿—散步，喝一杯，与同事聊天，或者做一些其他事情。有时，当您停止思考问题时，解决方案就会神奇地出现，即使没有，在感到神清气爽时也可以轻松解决问题。

### 您有有效的HTML和CSS吗？



浏览器希望您的CSS和HTML能够正确编写，但是浏览器也非常宽容，即使标记或样式表中有错误，浏览器也会尽力显示您的网页。如果您的代码有错误，浏览器需要猜测您的意思，并且可能会对您的想法做出不同的决定。此外，两种不同的浏览器可能会以两种不同的方式来解决该问题。因此，一个好的第一步是通过验证器运行HTML和CSS，以获取并修复所有错误。

- [CSS验证器](https://jigsaw.w3.org/css-validator/)
- [HTML验证器](https://validator.w3.org/)

### 被测试的浏览器是否支持该属性和值？



浏览器只是忽略他们不了解的CSS。如果您正在测试的浏览器不支持您正在使用的属性或值，则不会有任何问题，但不会应用CSS。DevTools通常会以某种方式突出显示不支持的属性和值。在下面的屏幕截图中，浏览器不支持的子网格值[`grid-template-columns`](https://developer.mozilla.org/en-US/docs/Web/CSS/grid-template-columns)。

![带有grid-template-columns的浏览器DevTools的图像：subgrid被划掉，因为不支持subgrid值。](https://mdn.mozillademos.org/files/16641/no-support.png)

您还可以查看MDN每个属性页底部的浏览器兼容性表。这些向您显示浏览器对该属性的支持，如果支持该属性的某些用法，而不支持其他用法，则经常会细分。下表显示了该[`shape-outside`](https://developer.mozilla.org/en-US/docs/Web/CSS/shape-outside)属性的兼容性数据。

找不到兼容性数据。请将“ css.shape-outside”（深度：1）的数据贡献给[MDN兼容性数据存储库](https://github.com/mdn/browser-compat-data)。

### 还有其他东西可以覆盖您的CSS吗？



在这里，您所学到的关于特异性的信息将非常有用。如果您有一些更具体的内容可以覆盖要执行的操作，则可以进入一个非常令人沮丧的游戏，尝试确定要执行的操作。但是，如上所述，DevTools将向您显示CSS正在应用什么，您可以弄清楚如何使新选择器足够具体，以覆盖它。

### 简化问题的测试案例



如果上述步骤未能解决问题，则您需要做更多调查。此时最好的做法是创建一个称为简化测试用例的东西。能够“减少问题”是一项非常有用的技能。这将帮助您在自己的代码和同事的代码中发现问题，还使您能够报告错误并更有效地寻求帮助。

简化的测试用例是一个代码示例，它以最简单的方式演示了该问题，并删除了无关的周围内容和样式。这通常意味着将有问题的代码从您的布局中取出，以制作一个仅显示该代码或功能的小示例。

要创建简化的测试用例：

1. 如果您的标记是动态生成的（例如通过CMS），请生成显示该问题的静态版本。诸如[CodePen之](https://codepen.io/)类的代码共享站点可用于托管简化的测试用例，因为它们可以在线访问，并且您可以轻松地与同事共享它们。您可以先在页面上执行“查看源代码”，然后将HTML复制到CodePen中，然后获取所有相关的CSS和JavaScript并将其包括在内。之后，您可以检查问题是否仍然明显。
2. 如果删除JavaScript不能解决问题，请不要包含JavaScript。如果删除JavaScript *确实*消除了问题，那么请尽可能多地删除JavaScript，并保留导致问题的原因。
3. 删除所有不会导致此问题的HTML。删除布局中的组件甚至主要元素。再次尝试减少代码数量，但仍然显示出该问题。
4. 删除所有不会影响此问题的CSS。

在执行此操作的过程中，您可能会发现导致问题的原因，或者至少能够通过删除特定问题来打开和关闭它。发现事物时，值得在代码中添加一些注释。如果您需要帮助，他们将向您显示帮助您的人。这很可能会为您提供足够的信息，以便能够搜索可能的听起来存在的问题和解决方法。

如果您仍在努力解决问题，那么减少测试用例可以通过发布到论坛或向同事展示来寻求帮助。如果可以证明您在寻求帮助之前已经完成了减少问题并准确确定问题根源的工作，那么您很有可能会获得帮助。一个更有经验的开发人员也许能够迅速发现问题并为您指明正确的方向，即使没有，您减少的测试用例也可以使他们快速查看并希望能够至少提供一些帮助。

如果您的问题实际上是浏览器中的错误，则还可以使用简化的测试用例向相关浏览器供应商提交错误报告（例如，在Mozilla的[bugzilla网站上](https://bugzilla.mozilla.org/)）。

随着对CSS的使用经验越来越丰富，您会发现发现问题的速度越来越快。但是，即使我们中最有经验的人有时也会发现自己想知道到底发生了什么。采取有条不紊的方法，减少测试用例，并向他人解释问题，通常会找到解决方法。