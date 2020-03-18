在本文中，我们将从更高层次上介绍JavaScript，并回答诸如“这是什么？”之类的问题。和“您可以用它做什么？”，并确保对JavaScript的目标满意。

| 先决条件： | 基本的计算机知识，对HTML和CSS的基本了解。              |
| :--------- | ------------------------------------------------------ |
| 目的：     | 要熟悉JavaScript是什么，它可以做什么以及如何适合网站。 |

## 高层次的定义

JavaScript是一种脚本语言或编程语言，可让您在网页上实现复杂的功能-每次网页要做的不只是坐在那里并显示静态信息供您查看-显示及时的内容更新，交互式地图，动画2D / 3D图形，滚动视频点唱机等-您可以打赌，可能涉及JavaScript。它是标准Web技术的第三层，在学习领域的其他部分中，我们更详细地介绍了其中的两个（[HTML](1/en-US/docs/Learn/HTML)和[CSS](1/en-US/docs/Learn/CSS)）。

![img](https://mdn.mozillademos.org/files/13502/cake.png)

- [HTML](1/en-US/docs/Glossary/HTML)是一种标记语言，我们可以用来构造Web内容并赋予其含义，例如定义段落，标题和数据表，或在页面中嵌入图像和视频。
- [CSS](1/en-US/docs/Glossary/CSS)是一种样式规则语言，我们用于将样式应用于HTML内容，例如设置背景颜色和字体，以及将内容布置在多列中。
- [JavaScript](1/en-US/docs/Glossary/JavaScript)是一种脚本语言，使您能够创建动态更新的内容，控制多媒体，动画图像以及几乎所有其他内容。（好吧，不是所有的东西，但是用几行JavaScript代码可以实现的效果令人惊奇。）

这三层很好地叠加在一起。让我们以一个简单的文本标签为例。我们可以使用HTML对其进行标记，以赋予其结构和用途：

```html
<p>Player 1: Chris</p>
```

![img](https://mdn.mozillademos.org/files/13422/just-html.png)

然后，我们可以将一些CSS添加到组合中，以使其看起来不错：

```css
p {
  font-family: 'helvetica neue', helvetica, sans-serif;
  letter-spacing: 1px;
  text-transform: uppercase;
  text-align: center;
  border: 2px solid rgba(0,0,200,0.6);
  background: rgba(0,0,200,0.3);
  color: rgba(0,0,200,0.6);
  box-shadow: 1px 1px 2px rgba(0,0,200,0.4);
  border-radius: 10px;
  padding: 3px 10px;
  display: inline-block;
  cursor: pointer;
}
```

![img](https://mdn.mozillademos.org/files/13424/html-and-css.png)

最后，我们可以添加一些JavaScript来实现动态行为：

```js
const para = document.querySelector('p');

para.addEventListener('click', updateName);

function updateName() {
  let name = prompt('Enter a new name');
  para.textContent = 'Player 1: ' + name;
}
```



尝试单击文本标签的最新版本以查看会发生什么（还要注意，您可以在GitHub上找到此演示—请查看[源代码](https://github.com/mdn/learning-area/blob/master/javascript/introduction-to-js-1/what-is-js/javascript-label.html)，或[实时运行它](http://mdn.github.io/learning-area/javascript/introduction-to-js-1/what-is-js/javascript-label.html)）！

JavaScript不仅可以做更多的事情-让我们更详细地研究一下。

## 那到底能做什么呢？

核心的客户端JavaScript语言包含一些常用的编程功能，这些功能使您可以执行以下操作：

- 将有用的值存储在变量内。例如，在上面的示例中，我们要求输入一个新名称，然后将该名称存储在名为的变量中`name`。
- 对文本片段的操作（在编程中称为“字符串”）。在上面的示例中，我们使用字符串“ Player 1：”并将其连接到`name`变量以创建完整的文本标签，例如“ Player 1：Chris”。
- 运行代码以响应网页上发生的某些事件。`click`在上面的示例中，我们使用了一个事件来检测单击按钮的时间，然后运行更新文本标签的代码。
- 以及更多！

然而，更令人兴奋的是基于客户端JavaScript语言构建的功能。所谓的**应用程序编程接口**（**API**）为您提供了在JavaScript代码中使用的额外超能力。

API是现成的代码构建块集，它们使开发人员能够实施原本难以实施或无法实施的程序。它们在编程方面的作用与现成的家具套件在住宅建筑中的作用相同–相比于自行设计，寻找并找到现成的面板，将现成的面板固定在一起并拧在一起以制成书架要容易得多。校正木材，将所有面板切成合适的尺寸和形状，找到正确尺寸的螺钉，*然后*将它们组装在一起制成书架。

它们通常分为两类。

![img](https://mdn.mozillademos.org/files/13508/browser.png)

**浏览器API**内置于您的Web浏览器中，并且能够公开来自周围计算机环境的数据，或执行有用的复杂操作。例如：

- 将[`DOM (Document Object Model) API`](1/en-US/docs/Web/API/Document_Object_Model)让您操作HTML和CSS，创建，删除和更改HTML，动态地应用新的样式到你的页面，等你看到一个弹出窗口出现在页面上每一次，或者一些新的内容显示（如我们上面看到我们例如简单的演示），那就是实际的DOM。
- 在[`Geolocation API`](1/en-US/docs/Web/API/Geolocation)获取地理信息。这就是[Google Maps](https://www.google.com/maps)能够找到您的位置并将其绘制在地图上的方式。
- 在[`Canvas`](1/en-US/docs/Web/API/Canvas_API)和[`WebGL`](1/en-US/docs/Web/API/WebGL_API)API允许你创建动画2D和3D图形。人们使用这些网络技术正在做一些令人惊奇的事情-请[参阅](http://webglsamples.org/)[Chrome实验](https://www.chromeexperiments.com/)和[webglsamples](http://webglsamples.org/)。
- [音频和视频API](1/en-US/Apps/Fundamentals/Audio_and_video_delivery)喜欢[`HTMLMediaElement`](1/en-US/docs/Web/API/HTMLMediaElement)并[`WebRTC`](1/en-US/docs/Web/API/WebRTC_API)允许您使用多媒体做真正有趣的事情，例如直接在网页上播放音频和视频，或从网络摄像头抓取视频并将其显示在其他人的计算机上（尝试通过我们的简单[Snapshot演示](http://chrisdavidmills.github.io/snapshot/)获得这个想法）。

**注意**：上面的许多演示都无法在较旧的浏览器中运行-实验时，最好使用Firefox，Chrome，Edge或Opera等现代浏览器运行代码。您需要考虑[跨浏览器测试](1/en-US/docs/Learn/Tools_and_testing/Cross_browser_testing)当您更接近交付生产代码（即，真正的客户将使用的真实代码）时，可以更详细地了解。

默认情况下，浏览器未内置**第三方API**，通常您必须从Web上的某个地方获取它们的代码和信息。例如：

- 在[Twitter的API](https://dev.twitter.com/overview/documentation)允许你做这样的事情在你的网站上显示你最新的鸣叫。
- 在[谷歌地图API](https://developers.google.com/maps/)和[OpenStreetMap的API](https://wiki.openstreetmap.org/wiki/API)允许你嵌入自定义地图到您的网站，以及其他类似的功能。

**注意**：这些API是高级的，因此在本模块中将不涉及任何这些API。您可以在我们的[客户端Web API模块中](1/en-US/docs/Learn/JavaScript/Client-side_web_APIs)找到更多有关这些的信息。

还有很多可用的！但是，请不要为此感到兴奋。在学习了24小时的JavaScript之后，您将无法构建下一个Facebook，Google Maps或Instagram-首先有很多基础知识。这就是为什么您在这里-让我们继续前进！

## 您页面上的JavaScript在做什么？

在这里，我们实际上将开始看一些代码，并在此过程中探索在页面中运行一些JavaScript时实际发生的情况。

让我们简要回顾一下在浏览器中加载网页时发生的情况（在我们的[CSS如何工作中]( 1

![img](https://mdn.mozillademos.org/files/13504/execution.png)

在将HTML和CSS组装在一起并放到网页中之后，JavaScript将由浏览器的JavaScript引擎执行。这样可以确保在JavaScript开始运行时，页面的结构和样式已经就位。

这是一件好事，因为JavaScript的一种非常普遍的用法是通过文档对象模型API（如上所述）来动态修改HTML和CSS以更新用户界面。如果JavaScript在影响HTML和CSS之前加载并尝试运行，则会发生错误。

### 浏览器安全



每个浏览器选项卡都是其自己的单独的存储桶（用于在其中运行代码）（这些存储桶在技术上称为“执行环境”）–这意味着在大多数情况下，每个选项卡中的代码都是完全独立运行的，并且一个选项卡中的代码无法直接运行影响另一个标签或另一个网站中的代码。这是一种很好的安全措施-如果不是这种情况，那么盗版者可能会开始编写代码来窃取其他网站和其他此类不良信息中的信息。

**注意**：有多种方法可以安全地在不同的网站/选项卡之间发送代码和数据，但是这些是我们在本课程中不会涉及的高级技术。

### JavaScript运行顺序



当浏览器遇到JavaScript块时，通常会按顺序从上到下运行它。这意味着您需要注意将事物放入的顺序。例如，让我们回到在第一个示例中看到的JavaScript块：

```js
const para = document.querySelector('p');

para.addEventListener('click', updateName);

function updateName() {
  let name = prompt('Enter a new name');
  para.textContent = 'Player 1: ' + name;
}
```

在这里，我们选择一个文本段落（第1行），然后在其上附加一个事件侦听器（第3行），以便在单击该段落时`updateName()`运行代码块（第5-8行）。的`updateName()`码块（这些类型的可重复使用的代码块中的被称为“功能”）要求用户为新的名称，然后插入该名称入款来更新显示。

如果你换的代码的前两行的顺序，将不再工作-相反，你会得到在返回错误[的浏览器开发者控制台](1/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools) - `TypeError: para is undefined`。这意味着该`para`对象尚不存在，因此我们无法向其添加事件侦听器。

**注意**：这是一个非常常见的错误-在尝试对它们进行处理之前，需要注意代码中引用的对象存在。

### 解释与编译代码



您可能会听到在编程环境中**解释**和**编译**的术语。在解释语言中，代码从上到下运行，并且运行代码的结果立即返回。在浏览器运行之前，您不必将代码转换为其他形式。该代码以其程序员友好的文本形式接收并直接从中进行处理。

另一方面，在计算机运行之前，已编译的语言会转换（编译）为另一种形式。例如，C / C ++被编译成汇编语言，然后由计算机运行。该程序以二进制格式执行，二进制格式是从原始程序源代码生成的。

JavaScript是一种轻量级的解释性编程语言。Web浏览器以其原始文本形式接收JavaScript代码，并从中运行脚本。从技术角度来看，大多数现代JavaScript解释器实际上都使用一种称为**即时编译的技术**来提高性能。在使用脚本时，JavaScript源代码被编译为更快的二进制格式，因此可以尽快运行。但是，JavaScript仍被认为是一种解释型语言，因为编译是在运行时而不是提前进行的。

两种语言都有其优势，但是我们现在不讨论它们。

### 服务器端与客户端代码



您可能还会听到术语“ **服务器****端**代码” 和“ **客户端**代码”，尤其是在Web开发的情况下。客户端代码是在用户计算机上运行的代码-当查看网页时，该页面的客户端代码将被下载，然后由浏览器运行并显示。在本模块中，我们明确地谈论**客户端JavaScript。

另一方面，服务器端代码在服务器上运行，然后将其结果下载并显示在浏览器中。流行的服务器端Web语言的示例包括PHP，Python，Ruby，ASP.NET和... JavaScript！JavaScript也可以用作服务器端语言

### 动态代码与静态代码



这个词**动态**用来描述了客户端JavaScript和服务器端语言-它是指以更新网页/应用程序的显示，以显示在不同情况下的不同事物的能力，因为需要产生新的内容。服务器端代码在服务器上动态生成新内容，例如从数据库中提取数据，而客户端JavaScript在客户端浏览器内部动态生成新内容，例如创建新的HTML表，并用服务器请求的数据填充，然后在显示给用户的网页中显示表格。在这两种情况下，含义略有不同，但是是相关的，并且这两种方法（服务器端和客户端）通常可以协同工作。

没有动态更新内容的网页称为**静态** -它始终显示相同的内容。

## 如何将JavaScript添加到页面？

JavaScript以类似于CSS的方式应用于HTML页面。CSS使用` <link>`元素将外部样式表应用于元素，将`<style>`元素将内部样式表应用于HTML，而JavaScript仅需要HTML世界中的一个朋友— `<script>`元素。让我们学习一下它是如何工作的。

### 内部JavaScript



1. 首先，为示例文件创建一个本地副本[apply-javascript.html](https://github.com/mdn/learning-area/blob/master/javascript/introduction-to-js-1/what-is-js/apply-javascript.html)。将其保存在合理的目录中。

2. 在Web浏览器和文本编辑器中打开文件。您会看到HTML创建了一个包含可单击按钮的简单网页。

3. 接下来，转到文本编辑器，然后在您的标题中添加以下内容-在结束`</head>`标记之前：

   ```html
   <script>
   
     // JavaScript goes here
   
   </script>
   ```

4. 现在，我们将在

   元素内添加一些JavaScript，以使页面做得更有趣-在“ // JavaScript go here”行下方添加以下代码：

   ```js
   document.addEventListener("DOMContentLoaded", function() {
     function createParagraph() {
       let para = document.createElement('p');
       para.textContent = 'You clicked the button!';
       document.body.appendChild(para);
     }
   
     const buttons = document.querySelectorAll('button');
   
     for(let i = 0; i < buttons.length ; i++) {
       buttons[i].addEventListener('click', createParagraph);
     }
   });
   ```

5. 保存文件并刷新浏览器-现在，您应该看到单击按钮时，将生成一个新段落并将其放置在下面。



### 外部JavaScript



这很好用，但是如果我们想将JavaScript放在外部文件中怎么办？让我们现在探讨这个。

1. 首先，在与示例HTML文件相同的目录中创建一个新文件。调用它`script.js`-确保它具有.js文件扩展名，因为这就是将其识别为JavaScript的方式。

2. 用`<script>`以下内容替换当前元素：

   ```html
   <script src="script.js" defer></script>
   ```

3. 在里面`script.js`，添加以下脚本：

   ```js
   function createParagraph() {
     let para = document.createElement('p');
     para.textContent = 'You clicked the button!';
     document.body.appendChild(para);
   }
   
   const buttons = document.querySelectorAll('button');
   
   for(let i = 0; i < buttons.length ; i++) {
     buttons[i].addEventListener('click', createParagraph);
   }
   ```

4. 保存并刷新浏览器，您将看到相同的内容！它的工作原理相同，但是现在我们将JavaScript放在了外部文件中。就组织代码并使之在多个HTML文件中可重用而言，这通常是一件好事。另外，无需在其中转储大量脚本，HTML更易于阅读。



### 内联JavaScript处理程序



请注意，有时您会遇到HTML内的一些实际JavaScript代码。它可能看起来像这样：

```js
function createParagraph() {
  let para = document.createElement('p');
  para.textContent = 'You clicked the button!';
  document.body.appendChild(para);
}
<button onclick="createParagraph()">Click me!</button>
```



该演示具有与前两节完全相同的功能，除了该`<button>`元素包括一个内联`onclick`处理程序以使函数在按下按钮时运行。

**但是，请不要这样做。**用JavaScript污染HTML是一种不好的做法，而且效率低下-您必须`onclick="createParagraph()"`在要应用JavaScript的每个按钮上都包含该属性。

使用纯JavaScript构造可让您使用一条指令选择所有按钮。我们上面用于实现此目的的代码如下所示：

```js
const buttons = document.querySelectorAll('button');

for(let i = 0; i < buttons.length ; i++) {
  buttons[i].addEventListener('click', createParagraph);
}
```

这可能比`onclick`属性长一点，但是它适用于所有按钮-无论页面上有多少按钮，也没有添加或删除多少按钮。无需更改JavaScript。

**注意**：尝试编辑您的的版本，`apply-javascript.html`然后在文件中添加更多按钮。重新加载时，应发现单击所有按钮都会创建一个段落。整洁吧？

### 脚本加载策略



在正确的时间获取脚本涉及很多问题。没有什么比看起来的那么简单！一个常见的问题是页面上的所有HTML均以显示顺序加载。如果您使用JavaScript来操纵页面上的元素（或更准确地说，是[Document Object Model](1/en-US/docs/Learn/JavaScript/Client-side_web_APIs/Manipulating_documents#The_document_object_model)），那么如果您在尝试执行操作的HTML之前加载并解析了JavaScript，则代码将无法工作。

在上面的代码示例中，在内部和外部示例中，在解析HTML主体之前，已在文档的开头加载并运行JavaScript。这可能会导致错误，因此我们使用了一些构造来解决它。

在内部示例中，您可以在代码周围看到以下结构：

```js
document.addEventListener("DOMContentLoaded", function() {
  ...
});
```

这是一个事件侦听器，它侦听浏览器的“ DOMContentLoaded”事件，该事件表示HTML主体已完全加载和解析。直到触发该事件后，该块中的JavaScript才会运行，因此避免了该错误（您将在本课程的后面部分中[了解事件](1/en-US/docs/Learn/JavaScript/Building_blocks/Events)）。

在外部示例中，我们使用一种更现代的JavaScript功能来解决该问题，即`defer`属性，该属性告诉浏览器`<scrpit>`在到达tag元素后继续下载HTML内容。

```js
<script src="script.js" defer></script>
```

在这种情况下，脚本和HTML将同时加载，并且代码将起作用。

**注意**：在外部情况下，我们不需要使用`DOMContentLoaded`事件，因为该`defer`属性为我们解决了问题。`defer`对于内部JavaScript示例，我们没有使用该解决方案，因为它`defer`仅适用于外部脚本。

这个问题的老式解决方案曾经是将脚本元素放在正文的底部（例如，紧接在`</body>`标记之前），以便在解析所有HTML之后加载它。该解决方案的问题在于，脚本的加载/解析被完全阻止，直到HTML DOM被加载为止。在具有许多JavaScript的大型网站上，这可能会导致严重的性能问题，从而降低您的网站速度。

#### 异步和延迟

实际上，我们可以使用两个现代功能来绕过阻止脚本的问题- `async`和`defer`（我们在上面看到）。让我们看一下两者之间的区别。

使用该`async`属性加载的脚本（请参见下文）将下载脚本，而不会阻止呈现页面，并在脚本完成下载后立即执行该脚本。您无法保证脚本将以任何特定的顺序运行，仅能确保它们不会阻止页面其余部分的显示。`async`当页面中的脚本彼此独立运行并且不依赖页面上的其他脚本时，最好使用该脚本。

例如，如果您具有以下脚本元素：

```html
<script async src="js/vendor/jquery.js"></script>

<script async src="js/script2.js"></script>

<script async src="js/script3.js"></script>
```

您不能依赖脚本加载的顺序。`jquery.js`可能在加载之前或之后`script2.js`，`script3.js`并且在这种情况下，依赖于那些脚本中的任何函数`jquery`都会产生错误，因为`jquery`在脚本运行时未定义。

`async`当您有许多后台脚本要加载时，应该使用它们，而您只想尽快将它们安装到位。例如，也许您要加载一些游戏数据文件，这是在游戏真正开始时需要的，但是现在您只想继续显示游戏介绍，标题和大厅，而不会被脚本加载所阻止。

使用该`defer`属性加载的脚本（见下文）将按照它们在页面中显示的顺序运行，并在下载脚本和内容后立即执行它们：

```html
<script defer src="js/vendor/jquery.js"></script>

<script defer src="js/script2.js"></script>

<script defer src="js/script3.js"></script>
```

具有该`defer`属性的所有脚本将按照它们在页面上显示的顺序加载。因此，在第二个示例中，我们可以确定`jquery.js`会在之前加载`script2.js`，`script3.js`并且`script2.js`会在之前加载`script3.js`。它们要等到页面内容全部加载后才能运行，如果脚本依赖于DOM（例如，它们修改页面上的多个元素之一），这将很有用。

总结一下：

- `async`与`defer`这两个指示浏览器下载一个单独的线程的脚本（S），而网页（DOM中，等）的其余部分被下载，所以网页加载时不会被脚本。
- 如果您的脚本应该立即运行并且没有任何依赖关系，请使用`async`。
- 如果您的脚本需要等待解析并且依赖于其他脚本和/或DOM，请使用加载它们，`defer` 并<script>按照您希望浏览器执行它们的顺序放置它们的相应元素。

## 注释

与HTML和CSS一样，可以在浏览器中将注释写入JavaScript代码中，而注释的存在只是为了向其他开发人员提供有关代码工作方式的说明（如果您返回代码，六个月后，不记得您做了什么）。注释非常有用，您应该经常使用它们，特别是对于大型应用程序。有两种类型：

- 在双斜杠（//）之后写入单行注释，例如

  ```js
  // I am a comment
  ```

- 多行注释写在字符串/ *和* /之间，例如

  ```js
  /*
    I am also
    a comment
  */
  ```

因此，例如，我们可以使用以下注释来注释上一个演示的JavaScript：

```js
// Function: creates a new paragraph and appends it to the bottom of the HTML body.

function createParagraph() {
  let para = document.createElement('p');
  para.textContent = 'You clicked the button!';
  document.body.appendChild(para);
}

/*
  1. Get references to all the buttons on the page in an array format.
  2. Loop through all the buttons and add a click event listener to each one.

  When any button is pressed, the createParagraph() function will be run.
*/

const buttons = document.querySelectorAll('button');

for (let i = 0; i < buttons.length ; i++) {
  buttons[i].addEventListener('click', createParagraph);
}
```

**注意**：通常，多注释总比少注释好，但是如果发现自己添加了很多注释来解释什么是变量（您的变量名称可能应该更直观），或者解释非常简单的操作（也许是您自己，代码过于复杂）。

## 