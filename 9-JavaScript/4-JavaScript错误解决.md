在上一篇文章中构建“猜数字”游戏时，您可能会发现它不起作用。别担心-本文旨在通过为您提供一些有关如何查找和修复JavaScript程序中的错误的提示，从而避免您因此类问题而烦恼。

| 先决条件： | 基本的计算机知识，对HTML和CSS的基本理解，对JavaScript的理解。 |
| :--------- | ------------------------------------------------------------ |
| 目的：     | 获得在您自己的代码中开始解决问题的能力和信心。               |

## 错误类型

一般来说，当您在代码中执行错误操作时，会遇到两种主要类型的错误：

- **语法错误**：这些是代码中的拼写错误，实际上导致该程序根本无法运行，或完全停止运行-通常也会向您提供一些错误消息。只要您熟悉正确的工具并知道错误消息的含义，这些通常就可以修复。
- **逻辑错误**：这些错误的语法实际上是正确的，但是代码不是您想要的，这意味着程序可以成功运行，但是给出错误的结果。这些错误通常比语法错误更难解决，因为通常没有错误消息可将您定向到错误源。

好的，这并不是*那么*简单-在深入研究时还有其他一些区别。但是以上分类将在您职业生涯的早期阶段进行。我们将继续探讨这两种类型。

## 一个错误的例子

首先，让我们回到数字猜谜游戏中-这次我们将探索引入一些故意错误的版本。转到Github，并将自己设置为[number-game-errors.html](https://github.com/mdn/learning-area/blob/master/javascript/introduction-to-js-1/troubleshooting/number-game-errors.html)的本地副本（请参见[此处实时运行](http://mdn.github.io/learning-area/javascript/introduction-to-js-1/troubleshooting/number-game-errors.html)）。

1. 首先，请在您喜欢的文本编辑器和浏览器中打开本地副本。
2. 尝试玩游戏-您会发现按下“提交猜测”按钮后，该按钮不起作用！

**注意**：您可能拥有自己的游戏示例版本，该版本无法正常运行，您可能需要修复该版本！我们仍然希望您阅读本文的版本，以便您可以在此处学习我们所教的技术。然后，您可以返回并尝试修复您的示例。

此时，让我们咨询开发人员控制台以查看其是否报告任何语法错误，然后尝试对其进行修复。您将在下面了解如何。

## 修复语法错误

在本课程的前面，我们让您在[开发人员工具JavaScript控制台中](1/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools)键入一些简单的JavaScript命令（如果您不记得如何在浏览器中打开它，请按照上一个链接查找操作方法）。更有用的是，只要将JavaScript内部存在语法错误并送入浏览器的JavaScript引擎，控制台就会向您显示错误消息。现在开始狩猎。

1. 转到`number-game-errors.html`打开的选项卡，然后打开JavaScript控制台。您应该看到以下几行的错误消息：![img](https://mdn.mozillademos.org/files/13496/not-a-function.png)

2. 这是一个非常容易找到的错误，浏览器为您提供了一些有用的信息来帮助您（上面的屏幕截图来自Firefox，但其他浏览器也提供类似的信息）。从左到右，我们有：

   - 红色的“ x”表示这是错误。
   - 一条错误消息，指出出了什么问题：“ TypeError：guessSubmit.addeventListener不是函数”
   - “学习更多”链接链接到MDN页面，该页面更详细地说明了此错误的含义。
   - JavaScript文件的名称，该文件链接到开发人员工具的“调试器”选项卡。如果您点击此链接，则会看到突出显示错误的确切行。
   - 错误所在的行号，以及该错误首次出现在该行的字符号。在这种情况下，我们得到第86行，字符编号3。

3. 如果我们在代码编辑器中查看第86行，则会发现以下行：

   ```js
   guessSubmit.addeventListener('click', checkGuess);
   ```

4. 错误消息显示“ guessSubmit.addeventListener不是函数”，这意味着JavaScript解释器无法识别我们正在调用的函数。通常，此错误消息实际上表示我们拼写错误。如果不确定某个语法的正确拼写，通常最好在MDN上查找该功能。当前最好的方法是使用您喜欢的搜索引擎搜索“功能*名称* ”。在这种情况下，这是一种节省时间的捷径：`addEventListener()`。

5. 因此，在查看此页面时，错误似乎是我们将函数名称拼写错误！请记住，JavaScript区分大小写，因此拼写或大小写的任何细微差异都会导致错误。更改`addeventListener`为`addEventListener`应该可以解决此问题。现在就做。

**注意**：有关此错误的更多详细信息，请参见我们的[TypeError：“ x”不是函数](1/en-US/docs/Web/JavaScript/Reference/Errors/Not_a_function)参考页。

### 第二轮语法错误



1. 保存页面并刷新，您应该看到错误已经消失。

2. 现在，如果您尝试输入一个猜测并按“提交猜测”按钮，您将看到...另一个错误！![img](https://mdn.mozillademos.org/files/13498/variable-is-null.png)

3. 这次在行78上报告的错误是“ TypeError：lowOrHi为null”。

   **注意**：`Null`是一个特殊值，表示“无”或“无值”。因此`lowOrHi`已经被声明和初始化，但是没有任何有意义的值-它没有类型或值。

   **注意**：页面加载后不会立即出现此错误，因为此错误发生在函数内部（在`checkGuess() { ... }`块内部）。正如您将在后面的[函数文章](1/en-US/docs/Learn/JavaScript/Building_blocks/Functions)中更详细地了解的那样，[函数](1/en-US/docs/Learn/JavaScript/Building_blocks/Functions)内部的代码与函数外部的代码在不同的范围内运行。在这种情况下，直到`checkGuess()`函数由第86行运行时，代码才运行，并且没有引发错误。

4. 看看第78行，您将看到以下代码：

   ```js
   lowOrHi.textContent = 'Last guess was too high!';
   ```

5. 此行试图`textContent`将`lowOrHi`常量的属性设置为文本字符串，但由于`lowOrHi`不包含预期的内容，因此无法正常工作。让我们看看为什么会这样-尝试在`lowOrHi`代码中搜索其他实例。您可以在JavaScript中找到的最早的实例在第48行：

   ```js
   const lowOrHi = document.querySelector('lowOrHi');
   ```

6. 此时，我们正在尝试使变量包含对文档HTML中元素的引用。让我们检查该值是否`null`在运行此行之后。在第49行上添加以下代码：

   ```js
   console.log(lowOrHi);
   ```

   **注意**：这`console.log()`是一个非常有用的调试功能，可将值打印到控制台。因此`lowOrHi`，一旦我们尝试在第48行中进行设置，它将立即将其值打印到控制台。

7. 保存并刷新，现在您应该`console.log()`在控制台中看到结果。![img](https://mdn.mozillademos.org/files/13494/console-log-output.png)当然，此时`lowOrHi`的值是`null`，因此第48行肯定存在问题。

8. 让我们考虑一下可能是什么问题。第48行使用一种

   ```
   document.querySelector()
   ```

   方法，通过使用CSS选择器选择元素来获取对元素的引用。进一步查找文件，我们可以找到相关段落：

   ```js
   <p class="lowOrHi"></p>
   ```

9. 因此，我们在这里需要一个以点（`.`）开头的类选择器，但是传递到第`querySelector()`48行的方法中的选择器没有点。这可能是问题所在！尝试更改`lowOrHi`为`.lowOrHi`第48行。

10. 尝试再次保存并刷新，您的`console.log()`语句应返回``我们想要的元素。！另一个错误已修复！您可以立即删除您的`console.log()`行，也可以保留以供以后参考（您的选择）。



### 第三轮语法错误



1. 现在，如果您再次尝试玩游戏，您应该会获得更大的成功-游戏应该玩的非常好，直到您通过猜测正确的数字或用尽所有的猜测结束游戏。
2. 到那时，游戏再次失败，并且吐出了我们一开始遇到的相同错误-“ TypeError：resetButton.addeventListener不是函数”！但是，这次它被列为来自第94行。
3. 查看第94行，很容易看出我们在这里犯了同样的错误。我们再次只需要更改`addeventListener`为`.addEventListener`。现在就做。

## 逻辑错误

在这一点上，游戏应该可以进行得很好，但是经过几次尝试之后，您无疑会注意到，您必须猜到的“随机数”始终为1。绝对不是我们希望游戏如何进行！

游戏逻辑肯定存在问题–游戏没有返回错误；只是播放不正确。

1. 搜索`randomNumber`变量，以及首先设置随机数的行。存储我们要在游戏开始时猜测的随机数的实例应该在第44行附近：

   ```js
   let randomNumber = Math.floor(Math.random()) + 1;
   ```

   在每个后续游戏之前都会产生随机数的代码大约在第113行：

2. ```js
   randomNumber = Math.floor(Math.random()) + 1;
   ```

3. 要检查这些行是否确实是问题所在，让我们

   ```
   console.log()
   ```

   再次转向我们的朋友-将以下行直接插入到以上两行的下方：

   ```js
   console.log(randomNumber);
   ```

4. 保存并刷新，然后玩一些游戏-在将`randomNumber`其记录到控制台的每个点上，您将看到它等于1。

### 通过逻辑工作



为了解决这个问题，让我们考虑一下这条线的工作方式。首先，我们调用`Math.random()`，它会生成一个介于0和1之间的随机十进制数，例如0.5675493843。

```js
Math.random()
```

接下来，我们通过调用的结果`Math.random()`通过`Math.floor()`，这轮传递给它下到最接近的整数数量。然后，我们向该结果添加1：

```html
Math.floor(Math.random()) + 1
```

在0和1之间向下舍入一个十进制数将始终返回0，因此对其加1始终将返回1。在将其舍入之前，需要将随机数乘以100。以下内容将为我们提供一个介于0到99之间的随机数：

```js
Math.floor(Math.random()*100);
```

因此，我们想要加1，以给我们一个1到100之间的随机数：

```js
Math.floor(Math.random()*100) + 1;
```

尝试像这样更新这两行，然后保存并刷新-游戏现在应该按照我们的预期进行了！

## 其他常见错误

您的代码中还会遇到其他常见错误。本节重点介绍其中的大多数内容。

### 语法错误：丢失；声明前



通常，此错误表示您在一行代码的末尾错过了分号，但有时可能更含糊。例如，如果我们在`checkGuess()`函数内更改此行：

```js
var userGuess = Number(guessField.value);
```

至

```js
var userGuess === Number(guessField.value);
```

抛出此错误是因为它认为您正在尝试执行其他操作。您应确保不要将赋值运算符（`=`）（将变量设置为等于值）与严格相等运算符（`===`）混淆，后者用于测试一个值是否等于另一个值，并返回`true`/ `false`结果。

**注意**：请参见我们的[SyntaxError：before语句](1/en-US/docs/Web/JavaScript/Reference/Errors/Missing_semicolon_before_statement)参考页，以获取有关此错误的更多详细信息。

### 程序总是说您赢了，无论您输入什么猜测



这可能是混合分配和严格相等运算符的另一种症状。例如，如果我们要在内部更改此行`checkGuess()`：

```js
if (userGuess === randomNumber) {
```

至

```js
if (userGuess = randomNumber) {
```

测试将始终返回`true`，从而导致程序报告游戏已获胜。小心！

### 语法错误：参数列表后缺少）



这很简单-通常意味着您在函数/方法调用的末尾错过了右括号。

**注意**：有关此错误的更多详细信息，请参见[参数列表](1/en-US/docs/Web/JavaScript/Reference/Errors/Missing_parenthesis_after_argument_list)参考页面[后的SyntaxError：missing）](1/en-US/docs/Web/JavaScript/Reference/Errors/Missing_parenthesis_after_argument_list)。

### SyntaxError：缺少：属性ID之后



此错误通常与格式不正确的JavaScript对象有关，但在这种情况下，我们设法通过更改

```js
function checkGuess() {
```

至

```js
function checkGuess( {
```

这导致浏览器认为我们正在尝试将函数的内容作为参数传递给函数。小心那些括号！

### SyntaxError：函数主体后缺少}



这很容易-通常意味着您从函数或条件结构中错过了一个大括号。我们通过删除`checkGuess()`函数底部附近的右花括号之一来得到此错误。

### SyntaxError：期望的表达式，获取了“ *string* ”或SyntaxError：未终止的字符串文字



这些错误通常表示您没有使用字符串值的开始或结束引号。在上面的第一个错误中，*字符串*将替换为浏览器发现的意外字符，而不是字符串开头的引号。第二个错误表示该字符串尚未以引号结尾。

对于所有这些错误，请考虑一下我们如何解决本演练中看到的示例。当出现错误时，查看给出的行号，转到该行，看看是否可以找出问题所在。请记住，该错误不一定会在该行上，并且该错误也可能不是由我们上面引用的完全相同的问题引起的！

**注意**：有关这些错误的更多详细信息，请参见我们的[SyntaxError：意外令牌](1/en-US/docs/Web/JavaScript/Reference/Errors/Unexpected_token)和[SyntaxError：未终止字符串文字](1/en-US/docs/Web/JavaScript/Reference/Errors/Unterminated_string_literal)参考页。

## 