至此，我们已经研究了CSS基础知识，如何设置文本样式以及如何设置和操作内容所在的框。现在是时候看看如何将框相对于视口放置在正确的位置，以及如何相互放置。我们已经介绍了必要的先决条件，因此我们现在可以深入了解CSS布局，查看不同的显示设置，诸如Flexbox，CSS网格和定位之类的现代布局工具，以及您可能仍想了解的一些传统技术。

## 先决条件

在开始本模块之前，您应该已经：

1. 基本了解HTML，如[HTML简介](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML)模块中所述。
2. 熟悉CSS基础，如[CSS简介中所述](https://developer.mozilla.org/en-US/docs/Learn/CSS/Introduction_to_CSS)。
3. 了解[框样式](https://developer.mozilla.org/en-US/docs/Learn/CSS/Styling_boxes)。

**注意**：如果您在无法创建自己的文件的计算机/平板电脑/其他设备上工作，则可以尝试（大多数）在线编码程序中的代码示例，例如[JSBin](http://jsbin.com/)或[Glitch](https://glitch.com/)。

## 导游

这些文章将提供有关CSS中可用的基本布局工具和技术的说明。在课程的最后，您将进行评估，以帮助您通过布置网页来检查对布局方法的理解。

- [CSS布局简介](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Introduction)

  本文将回顾我们在先前模块中已经涉及的一些CSS布局功能，例如不同的[`display`](https://developer.mozilla.org/en-US/docs/Web/CSS/display)值，并介绍我们将在本模块中介绍的一些概念。

- [正常流量](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Normal_Flow)

  网页上的元素会按照*正常的流程进行*布局-直到我们做出一些更改为止。本文介绍了正常流量的基础知识，以此作为学习如何更改流量的基础。

- [弹性盒](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Flexbox)

  [Flexbox](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout/Using_flexbox_to_lay_out_web_applications) 是一种用于按行或列布局项目的一[维布](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout/Using_flexbox_to_lay_out_web_applications)局方法。物品可以弯曲以填充额外的空间，并收缩以适合较小的空间。本文介绍了所有基础知识。学习[完](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Flexbox_skills)本指南后，您可以[测试您的flexbox技能，](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Flexbox_skills)以[在继续学习](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Flexbox_skills)之前检查您的理解。

- [格网](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Grids)

  CSS Grid Layout是Web的二维布局系统。它使您可以按行和列对内容进行布局，并具有许多功能，可轻松构建复杂的布局。本文将为您提供开始布局页面所需的全部知识，然后在继续之前[测试您的网格技能](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Grid_skills)。

- [浮点数](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Floats)

  该[`float`](https://developer.mozilla.org/en-US/docs/Web/CSS/float)属性最初是用于在文本块内浮动图像，后来成为在网页上创建多列布局的最常用工具之一。如本文所述，随着Flexbox和Grid的出现，它现在已恢复其原始用途。

- [定位](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Positioning)

  定位使您可以将元素从常规文档布局流程中移出，并使它们的行为有所不同，例如，一个元素相互叠放，或始终保留在浏览器视口中的同一位置。本文介绍了不同的[`position`](https://developer.mozilla.org/en-US/docs/Web/CSS/position)值，以及如何使用它们。

- [多列布局](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Multiple-column_Layout)

  多列布局规范为您提供了一种将内容按列排列的方法，就像您在报纸上看到的那样。本文介绍了如何使用此功能。

- [响应式设计](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Responsive_Design)

  随着启用Web的设备上出现越来越多的屏幕尺寸，响应式Web设计（RWD）的概念也出现了：一组允许Web页面更改其布局和外观以适应不同的屏幕宽度，分辨率等的做法。这个想法改变了我们为多设备网络设计的方式，在本文中，我们将帮助您了解掌握该网络所需的主要技术。

- [媒体查询入门指南](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Media_queries)

  该**CSS媒体查询**为您提供了一种方式，只有当浏览器和设备环境匹配您指定，例如规则“视口宽度大于480个像素”应用CSS。媒体查询是响应式网页设计的关键部分，因为它允许您根据视口的大小来创建不同的布局，但是它们也可以用于检测有关您的网站所运行的环境的其他信息，例如用户正在使用触摸屏而不是鼠标。在本课程中，您将首先学习媒体查询中使用的语法，然后在一个工作示例中继续使用它们，该示例显示了如何使简单的设计响应。

- [旧版布局方法](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Legacy_Layout_Methods)

  网格系统是CSS布局中非常常用的功能，在CSS网格布局之前，它们往往是使用浮点数或其他布局功能来实现的。您可以将布局想象为一定数量的列（例如4、6或12），然后将内容列放入这些假想的列中。在本文中，我们将探讨这些旧方法的工作方式，以便您了解在旧项目中工作时如何使用它们。

- [支持旧版浏览器](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Supporting_Older_Browsers)

  在本模块中，我们建议使用Flexbox和Grid作为设计的主要布局方法。但是，您网站的访问者会使用较旧的浏览器，或者不支持您使用的方法的浏览器。网络上总是如此-随着新功能的开发，不同的浏览器将对不同的事物进行优先级排序。本文介绍了如何使用现代Web技术，而又不会锁定使用旧技术的用户。

- [评估：基本布局理解](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Fundamental_Layout_Comprehension)

  通过布置网页来评估您对不同布局方法知识的评估。