在JavaScript中，大多数事物都是对象，从诸如字符串和数组之类的核心JavaScript功能到基于JavaScript 的浏览器[API](https://developer.mozilla.org/en-US/docs/Glossary/API)都是如此。您甚至可以创建自己的对象，以将相关功能和变量封装到有效的程序包中，并充当方便的数据容器。如果您想进一步了解该语言，JavaScript的基于对象的性质很重要，因此，我们提供了此模块来帮助您。在这里，我们详细地讲授对象理论和语法，然后介绍如何创建自己的对象。

## 先决条件

在开始本模块之前，您应该对[HTML](https://developer.mozilla.org/en-US/docs/Glossary/HTML)和[CSS](https://developer.mozilla.org/en-US/docs/Glossary/CSS)有所了解。您通过建议的工作[介绍HTML](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Introduction)和[CSS简介](https://developer.mozilla.org/en-US/docs/Learn/CSS/Introduction_to_CSS)开始于JavaScript之前模块。

在详细研究JavaScript对象之前，您还应该熟悉JavaScript基础。在尝试该模块之前，请完成[JavaScript的第一步](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/First_steps)和[JavaScript构建块](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks)。

**注意**：如果您在无法创建自己的文件的计算机/平板电脑/其他设备上工作，则可以尝试（大多数）在线编码程序中的代码示例，例如[JSBin](http://jsbin.com/)或[Glitch](https://glitch.com/)。

## 导游

- [对象基础](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Basics)

  在第一篇介绍JavaScript对象的文章中，我们将介绍基本的JavaScript对象语法，并重新访问本课程前面已经看过的一些JavaScript功能，重申一个事实，即您已经处理了许多功能。实际上是对象。

- [面向对象的面向初学者的JavaScript](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object-oriented_JS)

  在不了解基础知识的情况下，我们现在将重点介绍面向对象的JavaScript（OOJS）-本文介绍了面向对象的编程（OOP）理论的基本观点，然后探讨了JavaScript如何通过构造函数来仿真对象类，以及如何创建对象实例。

- [对象原型](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object_prototypes)

  原型是JavaScript对象彼此继承特征的机制，并且它们与经典的面向对象编程语言中的继承机制不同。在本文中，我们探讨了这种差异，解释了原型链如何工作，并研究了原型属性如何用于将方法添加到现有构造函数中。

- [JavaScript的继承](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Inheritance)

  现在说明了OOJS的大多数细节，本文介绍了如何创建从其“父”类继承特征的“子”对象类（构造函数）。此外，我们还提供有关何时何地使用OOJS的一些建议。

- [使用JSON数据](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/JSON)

  JavaScript Object Notation（JSON）是一种基于文本的标准格式，用于基于JavaScript对象语法表示结构化数据，该格式通常用于表示和传输网站上的数据（即，将一些数据从服务器发送到客户端，因此它可以在网页上显示）。您会经常遇到它，因此在本文中，我们为您提供了使用JavaScript使用JSON所需的全部功能，包括解析JSON，以便您可以访问其中的数据项并编写自己的JSON。

- [对象构建实践](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Object_building_practice)

  在先前的文章中，我们研究了所有基本的JavaScript对象理论和语法细节，为您提供了坚实的基础。在本文中，我们将深入实践练习，为您提供更多构建自定义JavaScript对象的实践，这些对象会产生有趣而丰富多彩的东西-一些彩色的弹跳球。