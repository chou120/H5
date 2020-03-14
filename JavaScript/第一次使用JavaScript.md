现在您已经了解了JavaScript的理论，以及可以使用它的方法，我们将通过一个完全实用的教程为您提供JavaScript基本功能的速成班。在这里，您将逐步构建一个简单的“猜数字”游戏。

| 先决条件： | 基本的计算机知识，对HTML和CSS的基本理解，对JavaScript的理解。 |
| :--------- | ------------------------------------------------------------ |
| 目的：     | 具备编写JavaScript的第一手经验，并且至少对编写JavaScript程序涉及的内容有基本的了解。 |

您不会期望您立即详细了解所有代码-我们只想向您介绍高级概念，并让您了解JavaScript（和其他编程语言）的工作方式。在后续文章中，您将更详细地介绍所有这些功能！

注意：您将在JavaScript中看到的许多代码功能与其他编程语言相同，例如函数，循环等。代码语法看起来有所不同，但概念仍基本相同。

## 像程序员一样思考

编程中最难学习的内容之一不是您需要学习的语法，而是如何应用它来解决现实世界中的问题。您需要像程序员一样开始思考-这通常涉及查看程序需要执行的描述，确定实现这些功能所需的代码功能以及如何使其协同工作。

这需要艰苦的工作，编程语法的经验和实践的结合，再加上一些创造力。您编写的代码越多，就会越好。我们不能保证您会在五分钟内培养“程序员的大脑”，但是在整个课程中，我们将为您提供很多机会像程序员一样练习思维。

考虑到这一点，让我们看一下我们将在本文中构建的示例，并回顾将其分解为有形任务的一般过程。



假设您的老板为您提供了以下有关创建此游戏的简介：

> 我希望您创建一个简单的猜数字游戏。它应该选择一个介于1到100之间的随机数，然后挑战玩家10个回合就猜出这个数字。每次转弯之后，应告知玩家是对还是错，如果错了，则猜测是太低还是太高。它还应该告诉玩家他们之前猜过的数字。一旦玩家猜对了，或者轮到他们用完了，游戏就会结束。游戏结束时，应该给玩家一个选项，让他重新开始玩。

看了这篇简短的文章后，我们要做的第一件事就是开始以尽可能多的程序员思维将其分解为简单的可操作任务：

1. 生成1到100之间的随机数。
2. 记录玩家所在的回合号码。从1开始
3. 为玩家提供一种猜测数字是多少的方法。
4. 提交猜测后，首先将其记录在某处，以便用户可以查看其先前的猜测。
5. 接下来，检查它是否正确。
6. 如果正确：
   1. 显示祝贺讯息。
   2. 阻止玩家输入更多的猜测（这会使游戏混乱）。
   3. 显示控件，允许玩家重新开始游戏。
7. 如果错了，玩家已经向左转：
   1. 告诉玩家他们错了。
   2. 让他们输入另一个猜测。
   3. 将匝数增加1。
8. 如果错了，并且玩家没有左转弯：
   1. 告诉玩家游戏结束了。
   2. 阻止玩家输入更多的猜测（这会使游戏混乱）。
   3. 显示控件，允许玩家重新开始游戏。
9. 游戏重新启动后，请确保完全重置了游戏逻辑和用户界面，然后返回到步骤1。

现在，让我们继续前进，研究如何将这些步骤转换为代码，构建示例，并逐步探索JavaScript功能。

### 初始设置



为了开始本教程，我们希望您制作一个[number-guessing-game-start.html](https://github.com/mdn/learning-area/blob/master/javascript/introduction-to-js-1/first-splash/number-guessing-game-start.html)文件的本地副本（[在此处实时查看](http://mdn.github.io/learning-area/javascript/introduction-to-js-1/first-splash/number-guessing-game-start.html)）。在文本编辑器和Web浏览器中将其打开。目前，您会看到一个简单的标题，说明段落和用于输入猜测的表格，但是该表格目前无法执行任何操作。

我们将添加所有代码的位置位于[``](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script)HTML底部的元素内：

```html
<script>

  // Your JavaScript goes here

</script>
```

### 添加变量以存储我们的数据



让我们开始吧。首先，在<script>元素内添加以下行：

```js
let randomNumber = Math.floor(Math.random() * 100) + 1;

const guesses = document.querySelector('.guesses');
const lastResult = document.querySelector('.lastResult');
const lowOrHi = document.querySelector('.lowOrHi');

const guessSubmit = document.querySelector('.guessSubmit');
const guessField = document.querySelector('.guessField');

let guessCount = 1;
let resetButton;
```

代码的这一部分设置了我们需要存储程序要使用的数据的变量和常量。变量基本上是值（例如数字或文本字符串）的容器。您可以使用关键字`let`（或`var`）和变量名创建一个变量（您将在[以后的文章中](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/First_steps/Variables#The_difference_between_var_and_let)了解有关关键字之间的区别的更多信息）。常量用于存储您不想更改的值，并使用关键字创建`const`。在这种情况下，我们使用常量来存储对用户界面部分的引用。其中一些文本可能会更改，但是引用的HTML元素保持不变。

您可以使用等号（`=`）后跟要为其赋值的值为变量或常量赋值。

在我们的示例中：

- 第一个变量- `randomNumber`被分配为1到100之间的随机数，使用数学算法计算得出。

- 前三个常量每个都用于存储对HTML中结果段落的引用，并用于稍后在代码中将值插入到段落中（请注意，它们如何在<div>元素内部，该元素本身用于选择所有三个稍后进行重置（当我们重新启动游戏时）：

  ```html
  <div class="resultParas">
    <p class="guesses"></p>
    <p class="lastResult"></p>
    <p class="lowOrHi"></p>
  </div>
  ```

- 接下来的两个常量存储对表单文本输入和提交按钮的引用，并用于控制稍后提交猜测。

  ```html
  <label for="guessField">Enter a guess: </label><input type="text" id="guessField" class="guessField">
  <input type="submit" value="Submit guess" class="guessSubmit">
  ```

- 我们的最后两个变量存储的猜测计数为1（用于跟踪玩家的猜测数），以及对尚不存在的重置按钮的引用（但稍后会引用）。

**注意**：从[下一篇文章](https://developer.mozilla.org/en-US/docs/user:chrisdavidmills/variables)开始，您将在本课程的后面部分中学习有关变量/常量的更多信息。

### 功能



接下来，在以前的JavaScript下方添加以下内容：

```js
function checkGuess() {
  alert('I am a placeholder');
}
```

函数是可重用的代码块，您可以编写一次并一次又一次地运行，从而节省了始终保持重复代码的需要。这真的很有用。定义函数的方法有很多，但是现在我们将集中在一个简单的类型上。在这里，我们使用关键字定义了一个函数`function`，后跟一个名称，并在其后加上括号。之后，我们放了两个花括号（`{ }`）。花括号内包含了我们每次调用该函数时要运行的所有代码。

当我们要运行代码时，我们键入函数的名称，后跟括号。

让我们现在尝试。保存代码并在浏览器中刷新页面。然后进入[开发人员工具JavaScript控制台](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools)，并输入以下行：

```js
checkGuess();
```

按下 Return/后Enter，您应该会看到一个警告消息：我是一个占位符“；我们已经在代码中定义了一个函数，该函数在每次调用它时都会创建一个警报。

**注意**：[在本课程的后面，](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Functions)您将学到更多有关功能[的知识](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Functions)。

### 经营者



JavaScript运算符使我们能够执行测试，进行数学运算，将字符串连接在一起以及其他类似的事情。

如果尚未这样做，请保存代码，在浏览器中刷新页面，然后打开[开发人员工具JavaScript控制台](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools)。然后，我们可以尝试键入下面显示的示例-完全按照所示在“示例”列中键入每个示例，在每个示例之后按Return/ Enter，然后查看它们返回的结果。

首先让我们看一下算术运算符，例如：

| 操作员 | 名称 | 例        |
| :----- | :--- | :-------- |
| `+`    | 加成 | `6 + 9`   |
| `-`    | 减法 | `20 - 15` |
| `*`    | 乘法 | `3 * 7`   |
| `/`    | 师   | `10 / 5`  |

您还可以使用`+`运算符将文本字符串连接在一起（在编程中，这称为*concatenation*）。尝试一次输入以下几行：

```js
let name = 'Bingo';
name;
let hello = ' says hello!';
hello;
let greeting = name + hello;
greeting;
```

还有一些可用的捷径运算符，称为增强[分配运算符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Assignment_Operators)。例如，如果您只想向现有文本字符串中添加一个新的文本字符串并返回结果，则可以执行以下操作：

```js
name += ' says hello!';
```

这相当于

```js
name = name + ' says hello!';
```

当我们运行对/错测试时（例如在条件语句内部-参见[下文](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/First_steps/A_first_splash#Conditionals)），我们使用[比较运算符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Comparison_Operators)。例如：

| 操作员 | 名称                     | 例                                                           |
| :----- | :----------------------- | :----------------------------------------------------------- |
| `===`  | 严格平等（完全一样吗？） | `5 === 2 + 4 // false 'Chris' === 'Bob' // false 5 === 2 + 3 // true 2 === '2' // false; number versus string ` |
| `!==`  | 非平等（不一样吗？）     | `5 !== 2 + 4 // true 'Chris' !== 'Bob' // true 5 !== 2 + 3 // false 2 !== '2' // true; number versus string ` |
| `<`    | 少于                     | `6 < 10 // true 20 < 10 // false`                            |
| `>`    | 比...更棒                | `6 > 10 // false 20 > 10  // true`                           |

### 有条件的



回到我们的`checkGuess()`功能，我认为可以肯定地说我们不希望它只发出一个占位符消息。我们希望它检查玩家的猜测是否正确，并做出适当的响应。

此时，请`checkGuess()`使用以下版本替换当前函数：

```js
function checkGuess() {
  let userGuess = Number(guessField.value);
  if (guessCount === 1) {
    guesses.textContent = 'Previous guesses: ';
  }
  guesses.textContent += userGuess + ' ';
 
  if (userGuess === randomNumber) {
    lastResult.textContent = 'Congratulations! You got it right!';
    lastResult.style.backgroundColor = 'green';
    lowOrHi.textContent = '';
    setGameOver();
  } else if (guessCount === 10) {
    lastResult.textContent = '!!!GAME OVER!!!';
    setGameOver();
  } else {
    lastResult.textContent = 'Wrong!';
    lastResult.style.backgroundColor = 'red';
    if(userGuess < randomNumber) {
      lowOrHi.textContent = 'Last guess was too low!';
    } else if(userGuess > randomNumber) {
      lowOrHi.textContent = 'Last guess was too high!';
    }
  }
 
  guessCount++;
  guessField.value = '';
  guessField.focus();
}
```

这是很多代码-！让我们遍历每个部分并说明其作用。

- 第一行（上面的第2行）声明了一个名为的变量`userGuess`，并将其值设置为在文本字段内输入的当前值。我们还通过内置`Number()`构造函数运行此值，只是要确保该值绝对是数字。

- 接下来，我们遇到第一个条件代码块（上面的3–5行）。条件代码块允许您根据某个条件是否为真来有选择地运行代码。它看起来有点像一个函数，但事实并非如此。条件块的最简单形式以关键字开头if，然后是一些括号，然后是大括号。括号内包含一个测试。如果测试返回true，则在花括号内运行代码。如果不是，那么我们就不这样做，然后继续进行下一部分代码。在这种情况下，测试正在测试guessCount变量是否等于1（即，这是否是玩家的首次尝试）：

  ```js
  guessCount === 1
  ```

  如果是这样，我们使猜测段落的文本内容等于“先前的猜测： “。如果没有，我们不会。

- 第6行将当前`userGuess`值附加到`guesses`段落的末尾，再加上一个空格，因此在显示的每个猜测之间将有一个空格。

- 下一个块（上面的8-24行）进行了一些检查：

  - 第一个`if(){ }`检查用户的猜测是否等于`randomNumber`我们JavaScript顶部的设定。如果是的话，则说明玩家猜对了并赢得了比赛，因此我们向玩家显示一条带有绿色的祝贺消息，清除“低/高猜测”信息框的内容，并运行一个名为的函数`setGameOver()`，稍后再讨论。
  - 现在，我们使用`else if(){ }`结构将另一个测试链接到最后一个测试的末尾。这一个检查此回合是否是用户的最后回合。如果是，则该程序执行与上一个块中相同的操作，除了使用游戏结束消息而不是祝贺消息。
  - 链接到该代码末尾的最后一个块（`else { }`）包含仅在其他两个测试均未返回true时运行的代码（即玩家猜对了，但剩下的猜测还多）。在这种情况下，我们告诉他们他们错了，然后我们执行另一个条件测试以检查猜测是高于还是低于答案，并显示适当的进一步消息以告诉他们更高或更低。

- 函数的最后三行（上面的26-28行）使我们准备好提交下一个猜测。我们在`guessCount`变量上加1，以便玩家用完自己的回合（`++`是一个递增操作-递增1），然后从表单文本字段中清空该值并将其再次聚焦，以准备输入下一个猜测。

### 大事记



在这一点上，我们已经实现了一个很好的`checkGuess()`函数，但是由于我们还没有调用它，它什么也不会做。理想情况下，我们想在按下“提交猜测”按钮时调用它，为此，我们需要使用一个**event**。事件是浏览器中发生的事情，例如单击按钮，加载页面，播放视频等，作为响应，我们可以运行代码块。侦听事件发生的结构称为**事件侦听器**，而响应事件触发而运行的代码块称为**事件处理程序**。

在`checkGuess()`函数下方添加以下行：

```js
guessSubmit.addEventListener('click', checkGuess);
```

在这里，我们向`guessSubmit`按钮添加了一个事件监听器。这是一种采用两个输入值（称为*arguments*）的方法-我们正在侦听的事件的类型（在此例中`click`为字符串），以及当事件发生时我们要运行的代码（在本例中为`checkGuess()`函数） 。请注意，在内部编写括号时，无需指定括号[`addEventListener()`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener)。

现在尝试保存并刷新代码，这样您的示例就可以工作了。现在唯一的问题是，如果您猜对了答案或猜对了，游戏将会中断，因为我们尚未定义`setGameOver()`游戏结束后应该运行的功能。让我们现在添加缺少的代码，并完成示例功能。

### 完成游戏功能



让我们将该`setGameOver()`函数添加到代码的底部，然后逐步完成。现在，在您的其余JavaScript下方添加此代码：

```js
function setGameOver() {
  guessField.disabled = true;
  guessSubmit.disabled = true;
  resetButton = document.createElement('button');
  resetButton.textContent = 'Start new game';
  document.body.appendChild(resetButton);
  resetButton.addEventListener('click', resetGame);
}
```

- 前两行通过将其禁用属性设置为来禁用表单文本输入和按钮`true`。这是必要的，因为如果我们不这样做，则用户可以在游戏结束后提交更多的猜测，这会使事情变得混乱。
- 接下来的三行生成一个新<button>元素，将其文本标签设置为“开始新游戏”，并将其添加到我们现有HTML的底部。
- 最后一行在我们的新按钮上设置了一个事件侦听器，以便在单击它时`resetGame()`运行一个名为的函数。

现在我们也需要定义此功能！将以下代码再次添加到JavaScript的底部：

```js
function resetGame() {
  guessCount = 1;

  const resetParas = document.querySelectorAll('.resultParas p');
  for (let i = 0 ; i < resetParas.length ; i++) {
    resetParas[i].textContent = '';
  }

  resetButton.parentNode.removeChild(resetButton);

  guessField.disabled = false;
  guessSubmit.disabled = false;
  guessField.value = '';
  guessField.focus();

  lastResult.style.backgroundColor = 'white';

  randomNumber = Math.floor(Math.random() * 100) + 1;
}
```

这个相当长的代码块将所有内容完全重置为游戏开始时的状态，因此玩家可以再试一次。它：

- 放`guessCount`回落至1。
- 从信息段落中清空所有文本。我们选择内的所有段落<div class="resultParas"></div>，然后遍历每个段落，将其设置`textContent`为" "（空字符串）。
- 从我们的代码中删除重置按钮。
- 启用表单元素，清空并聚焦文本字段，以准备输入新的猜测。
- 从`lastResult`段落中删除背景色。
- 生成一个新的随机数，这样您就不仅可以再次猜测相同的数字！

**在这一点上，您应该有一个可以正常运行的（简单）游戏-祝贺您！**

现在，我们剩下要做的就是讨论您已经看到的其他一些重要的代码功能，尽管您可能还没有意识到。

### 循环



我们需要更详细地研究上述代码的一部分是[for](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for)循环。循环是编程中非常重要的概念，它使您可以反复运行一段代码，直到满足特定条件为止。

首先，请再次转到[浏览器开发人员工具JavaScript控制台](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools)，然后输入以下内容：

```js
for (let i = 1 ; i < 21 ; i++) { console.log(i) }
```

发生了什么？号码1个 至 20在您的控制台中打印出来。这是因为循环。甲`for`环采用了三个输入的值（参数）：

1. **起始值**：在这种情况下，我们将从1开始计数，但是它可以是您喜欢的任何数字。您也可以`i`用任何喜欢的名称替换字母，但`i`由于其简短易记，因此被用作惯例。
2. **退出条件**：这里已指定`i < 21`-循环将继续进行，直到`i`不小于21。当`i`达到21时，循环将不再运行。
3. **增量器**：我们已指定`i++`，表示“将i加1”。循环将针对的每个值运行一次`i`，直到`i`达到21的值（如上所述）。在这种情况下，我们只需`i`在每次迭代中将out 的值输出到控制台即可[`console.log()`](https://developer.mozilla.org/en-US/docs/Web/API/Console/log)。

现在，让我们看一下猜数字游戏中的循环-在`resetGame()`函数内部可以找到以下内容：

```js
const resetParas = document.querySelectorAll('.resultParas p');
for (let i = 0 ; i < resetParas.length ; i++) {
  resetParas[i].textContent = '';
}
```

这段代码<div class="resultParas"></div>使用该[`querySelectorAll()`](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelectorAll)方法创建了一个变量，其中包含所有内部段落的列表，然后循环遍历每个段落，删除每个段落的文本内容。

### 关于对象的小讨论



在进行此讨论之前，让我们再添加一个最终的改进。`let resetButton;`在JavaScript顶部附近的行下方添加以下行，然后保存文件：

```js
guessField.focus();
```

此行使用此[`focus()`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/focus)方法，将在<input>页面加载后立即将文本光标自动放入文本字段，这意味着用户可以立即开始键入他们的第一个猜测，而不必先单击表单字段。这只是一个很小的补充，但它提高了可用性-为用户提供了一个很好的视觉线索，以了解他们在玩游戏时需要做什么。

让我们更详细地分析这里发生的事情。在JavaScript中，一切都是对象。对象是存储在单个分组中的相关功能的集合。您可以创建自己的对象，但这是相当高级的，我们将在本课程的稍后部分进行介绍。现在，我们将简要讨论浏览器包含的内置对象，这些对象使您可以做很多有用的事情。

在这种情况下，我们首先创建一个`guessField`常量，该常量存储对HTML中文本输入表单字段的引用-在代码顶部附近的声明中可以找到以下行：

```js
const guessField = document.querySelector('.guessField');
```

为了获得参考，我们使用[`querySelector()`](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector)了[`document`](https://developer.mozilla.org/en-US/docs/Web/API/Document)对象的方法。`querySelector()`需要一条信息—一个[CSS选择器](https://developer.mozilla.org/en-US/docs/Learn/CSS/Introduction_to_CSS/Selectors)，用于选择要引用的元素。

因为`guessField`现在包含对<input>元素的引用，所以它现在可以访问许多属性（基本上是存储在对象内部的变量，其中一些不能更改其值）和方法（基本上是存储在对象内部的函数）。输入元素可用的一种方法是`focus()`，因此我们现在可以使用此行来集中文本输入：

```js
guessField.focus();
```

不包含对表单元素的引用的变量将无法`focus()`使用。例如，`guesses`常量包含对<p>元素的引用，而`guessCount`变量包含数字。

### 玩浏览器对象



让我们玩一些浏览器对象。

1. 首先，在浏览器中打开程序。

2. 接下来，打开[浏览器开发人员工具](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools)，并确保JavaScript控制台选项卡已打开。

3. 键入`guessField`到控制台和控制台显示你的变量中包含[``](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input)的元素。您还会注意到，控制台会自动完成执行环境中存在的对象的名称，包括变量！

4. 现在输入以下内容：

   ```js
   guessField.value = 'Hello';
   ```

   该value属性表示输入到文本字段中的当前值。您会看到通过输入此命令，我们已更改了文本字段中的文本！

5. 现在尝试`guesses`在控制台中键入并按回车键。控制台将向您显示该变量包含一个<p>元素。

6. 现在尝试输入以下行：

   ```js
   guesses.value
   ```

   浏览器返回undefined，因为段落没有该value属性。

7. 要更改段落内的文本，您需要使用`textContent`属性。尝试这个：

   ```js
   guesses.textContent = 'Where is my paragraph?';
   ```

8. 现在来一些有趣的东西。尝试一一输入以下几行：

   ```js
   guesses.style.backgroundColor = 'yellow';
   guesses.style.fontSize = '200%';
   guesses.style.padding = '10px';
   guesses.style.boxShadow = '3px 3px 6px black';
   ```

   页面上的每个元素都有一个`style`属性，该属性本身包含一个对象，该对象的属性包含应用于该元素的所有内联CSS样式。这使我们能够使用JavaScript在元素上动态设置新的CSS样式。