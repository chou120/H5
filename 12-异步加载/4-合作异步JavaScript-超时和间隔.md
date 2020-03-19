本教程介绍了JavaScript在设定的时间段后或以固定的时间间隔（例如，每秒设定的次数）后可用于异步运行代码的传统方法，讨论了它们的用处并考虑了其固有的问题。

| 先决条件： | 基本的计算机知识，对JavaScript基础的合理理解。 |
| :--------- | ---------------------------------------------- |
| 目的：     | 了解异步循环和间隔以及它们的作用。             |

## 介绍

长期以来，Web平台已为JavaScript程序员提供了许多功能，使他们可以在经过一定时间间隔后异步执行代码，并重复异步执行代码块，直到您将其停止为止。

这些功能是：

- `setTimeout()`

  在指定的时间过去之后，执行一次指定的代码块。

- `setInterval()`

  在每次调用之间以固定的时间延迟重复执行指定的代码块。

- `requestAnimationFrame()`

  的现代版本`setInterval()`。在浏览器接下来重新粉刷显示内容之前执行指定的代码块，从而使动画可以以合适的帧速率运行，而不管其运行的环境如何。

这些函数设置的异步代码在主线程上运行（经过指定的计时器后）。

重要的是要知道您可以（并且经常会）在`setTimeout()`调用执行之前或的迭代之间运行其他代码`setInterval()`。根据这些操作的处理器密集程度，它们可以进一步延迟异步代码，因为任何异步代码仅*在*主线程可用*后*才会执行。（换句话说，当堆栈为空时。）随着本文的逐步进行，您将学到更多有关此问题的信息。

无论如何，这些功能用于在网站或应用程序上运行恒定的动画和其他后台处理。在以下各节中，我们将向您展示如何使用它们。

## setTimeout（）

如前所述，`setTimeout()`在经过指定的时间后执行一次特定的代码块。它采用以下参数：

- 要运行的功能，或对其他位置定义的功能的引用。
- 一个数字，表示执行代码之前要等待的时间间隔（以毫秒为单位（1000毫秒等于1秒））。如果您指定一个值`0`（或简单地忽略该值），该函数将尽快运行。（请参阅下面的注释，了解为什么它“尽快”而不是“立即”运行。）有关为什么以后可能要这样做的更多信息。
- 零个或多个值，表示要在函数运行时传递给该函数的任何参数。

**注：** 在规定的时间量（或者延迟）是**不** 将*保证的时间*来执行，而是*最小时间*来执行。在主线程上的堆栈为空之前，传递给这些函数的回调将无法运行。

结果，类似的代码`setTimeout(fn, 0)` 将在堆栈为空时立即执行，**而不是**立即执行。如果您执行这样的代码，`setTimeout(fn, 0)`但是在运行一个从1到100亿的循环之后立即执行 ，那么您的回调将在几秒钟后执行。

在以下示例中，浏览器将在执行匿名功能之前等待两秒钟，然后将显示警报消息

```js
let myGreeting = setTimeout(function() {
  alert('Hello, Mr. Universe!');
}, 2000)
```

您指定的功能不必是匿名的。您可以给函数命名，甚至在其他地方定义它，然后将函数引用传递给`setTimeout()`。以下两个版本的代码片段与第一个版本等效：

```js
// With a named function
let myGreeting = setTimeout(function sayHi() {
  alert('Hello, Mr. Universe!');
}, 2000)

// With a function defined separately
function sayHi() {
  alert('Hello Mr. Universe!');
}

let myGreeting = setTimeout(sayHi, 2000);
```

例如，如果您有一个需要同时从超时和响应事件中调用的函数，则该功能很有用。但这也可以帮助保持代码整洁，尤其是在超时回调超过几行代码的情况下。

`setTimeout()`返回一个标识符值，该标识符值可用于稍后引用超时，例如何时要停止超时。

### 将参数传递给setTimeout（）函数



您想要传递给正在内部运行的函数的任何参数，都`setTimeout()`必须作为附加参数传递给列表末尾。

例如，您可以重构先前的函数，以便对传递给它的任何人的名字打个招呼：

```js
function sayHi(who) {
  alert(`Hello ${who}!`);
}
```

现在，您可以将联系人的姓名`setTimeout()`作为第三个参数传递给 呼叫：

```js
let myGreeting = setTimeout(sayHi, 2000, 'Mr. Universe');
```

### 清除超时



最后，如果已创建超时，则可以通过调用`clearTimeout()`，将调用的标识符`setTimeout()`作为参数传递给指定的时间，然后在指定的时间过去之前将其取消。因此，要取消上述超时，您可以这样做：

```js
clearTimeout(myGreeting);
```

**注意**：请参见参考资料`greeter-app.html`，其中包含一个稍微复杂的演示，该演示使您可以在表单中设置向您打招呼的人的姓名，并使用单独的按钮取消问候语。

## setInterval（）

`setTimeout()`当您需要在一段设定的时间后运行一次代码时，它可以完美地工作。但是，如果您需要一遍又一遍地运行代码（例如动画），会发生什么？

这就是其中的`setInterval()`用处。它的工作方式与相似`setTimeout()`，不同之处在于，您作为第一个参数传递的函数将以不小于第二个参数指定的毫秒数（而不是一次）重复执行。您还可以将正在执行的功能所需的任何参数作为`setInterval()`调用的后续参数传递。

让我们来看一个例子。以下函数创建一个新`Date()`对象，使用提取一个时间字符串`toLocaleTimeString()`，然后在UI中显示它。然后，它使用每秒运行一次功能`setInterval()`，从而创建每秒更新一次的数字时钟的效果

```js
function displayTime() {
   let date = new Date();
   let time = date.toLocaleTimeString();
   document.getElementById('demo').textContent = time;
}

const createClock = setInterval(displayTime, 1000);
```

就像一样`setTimeout()`，会`setInterval()`返回一个标识值，您以后可以在需要清除间隔时使用它。

### 清理间隔



`setInterval()`保持永远运行任务，除非您对此做任何事情。您可能需要一种停止此类任务的方法，否则，当浏览器无法完成该任务的任何其他版本时，或者该任务正在处理的动画完成时，您可能最终会出错。您可以使用与停止超时相同的方式进行此操作-通过将`setInterval()`调用返回的标识符传递给`clearInterval()`函数：

```js
const myInterval = setInterval(myFunction, 2000);

clearInterval(myInterval);
```

#### 主动学习：创建自己的秒表！

综上所述，我们为您带来了挑战。复制我们的`setInterval-clock.html`示例，然后对其进行修改以创建自己的简单秒表。

您需要像以前一样显示时间，但是在此示例中，您需要：

- 一个“开始”按钮，使秒表开始运行。
- 一个“停止”按钮可以暂停/停止它。
- “重置”按钮可将时间重置为`0`。
- 时间显示将显示经过的秒数，而不是实际时间。

以下是一些提示：

- 您可以随意构建和设置按钮标记的样式；只要确保您使用语义HTML，并使用钩子就可以使用JavaScript来获取按钮引用。
- 您可能想要创建一个以开头的变量`0`，然后使用一个恒定循环每秒递增1。
- `Date()`就像我们在版本中所做的那样，不使用对象创建此示例会更容易，但准确性较低-您无法保证回调会在`1000`ms 之后触发。一种更准确的方法是运行`startTime = Date.now()`以获取用户单击“开始”按钮时的确切时间戳，然后执行操作`Date.now() - startTime`以获取单击“开始”按钮后的毫秒数。
- 您还希望将小时，分钟和秒数计算为单独的值，然后在每次循环迭代后将它们一起显示在字符串中。从第二个计数器，您可以算出每个。
- 您将如何计算它们？考虑一下：
  - 每小时的秒数是`3600`。
  - 分钟数是除掉所有小时后剩下的秒数`60`。
  - 秒数将是所有分钟都被删除后剩余的秒数。
- 如果数量小于`10`，则需要在显示值上包含前导零。因此，它看起来更像是传统的时钟/手表。
- 要暂停秒表，您需要清除间隔。要重置它，您需要将计数器设置回`0`，清除间隔，然后立即更新显示。
- 您可能应该在按一下开始按钮后将其禁用，然后在停止/重置后再次启用它。否则，多次按下开始按钮将对`setInterval()`时钟施加多个s，从而导致错误的行为。

## 有关setTimeout（）和setInterval（）的注意事项

与`setTimeout()`和一起使用时，需要牢记一些注意事项`setInterval()`。现在让我们回顾一下。

### 递归超时



还有另一种使用方式`setTimeout()`：您可以递归调用它以重复运行相同的代码，而不是使用`setInterval()`。

下面的示例使用递归`setTimeout()`方法每`100`毫秒运行一次传递的函数：

```js
let i = 1;

setTimeout(function run() {
  console.log(i);
  i++;
  setTimeout(run, 100);
}, 100);
```

将上面的示例与下面的示例进行比较- `setInterval()`可以达到相同的效果：

```js
let i = 1;

setInterval(function run() {
  console.log(i);
  i++
}, 100);
```

#### 递归`setTimeout()`和`setInterval()`差异如何？

上面代码的两个版本之间的区别是微妙的。

- 递归`setTimeout()`保证两次执行之间的相同延迟。（例如，`100`在上面的情况下为ms。）代码将运行，然后等待`100`毫秒才能再次运行-因此，无论代码运行多长时间，间隔都将是相同的。
- 该示例使用的`setInterval()`功能有所不同。您选择的间隔*包括*执行您要在其中运行的代码所花费`40`的时间。比方说，代码要花费毫秒才能运行-间隔最终只有`60`毫秒。
- `setTimeout()`递归使用时，每个迭代可以在运行下一个迭代之前计算出不同的延迟。换句话说，第二个参数的值可以指定不同的时间（以毫秒为单位），以等待再次运行代码。

当您的代码有可能花费比指定的时间间隔更长的时间运行时，最好使用递归`setTimeout()`-无论代码执行多长时间，这都将使两次执行之间的时间间隔保持恒定，并且您不会得到错误。

### 立即超时



将`0`用作值用于`setTimeout()`计划指定的回调函数尽快执行，但仅在运行主代码线程之后执行。

例如，下面的代码输出一个包含的警报`"Hello"`，然后`"World"`在您单击第一个警报上的OK后立即包含一个警报。

```js
setTimeout(function() {
  alert('World');
}, 0);

alert('Hello');
```

如果您希望设置一个代码块以便在所有主线程完成运行后立即运行，这将很有用-将其放在async事件循环中，这样它将随后直接运行。

### 使用clearTimeout（）或clearInterval（）进行清除



`clearTimeout()`并且`clearInterval()`都使用相同的条目列表进行清除。足够有趣的是，这意味着您可以使用任一方法清除`setTimeout()`或`setInterval()`。

为了保持一致性，您应该使用`clearTimeout()`清除`setTimeout()`条目和`clearInterval()`清除`setInterval()`条目。这将有助于避免混乱。

## requestAnimationFrame（）

`requestAnimationFrame()`是一种专门的循环功能，旨在在浏览器中高效运行动画。它基本上是的现代版本`setInterval()`-它在浏览器接下来重新粉刷显示内容之前执行指定的代码块，从而允许动画以合适的帧速率运行，而不管其运行的环境如何。

它是根据感知到的问题而创建的，`setInterval()`例如，该问题并非以针对设备优化的帧速率运行，有时会丢帧，即使选项卡不是活动选项卡或动画从页面上滚动也会继续运行等

该方法将重画之前要调用的回调作为参数。这是您将看到的一般模式：

```js
function draw() {
   // Drawing code goes here
   requestAnimationFrame(draw);
}

draw();
```

这个想法是要定义一个函数，在其中更新动画（例如，移动精灵，更新乐谱，刷新数据等）。然后，您调用它以开始该过程。在功能块的末尾，您`requestAnimationFrame()`以传递的函数引用作为参数进行调用，这指示浏览器在下一次显示重新绘制时再次调用该函数。然后，此操作将连续运行，因为代码是`requestAnimationFrame()`递归调用的。

**注意**：如果要执行某种简单的恒定DOM动画，则[CSS动画]( /CSS_Animations)可能会更快。它们由浏览器的内部代码而不是JavaScript直接计算。

但是，如果您正在做一些更复杂的事情，并且涉及在DOM内部无法直接访问的对象（例如[2D Canvas API或[WebGL对象），`requestAnimationFrame()`则在大多数情况下是更好的选择。

### 动画的运行速度有多快？



动画的平滑度直接取决于动画的帧频，并且以每秒帧数（fps）为单位进行度量。这个数字越高，动画看起来就越平滑。

由于大多数屏幕的刷新率均为60Hz，因此使用Web浏览器时，您可以争取的最快帧速率为60帧/秒（FPS）。但是，更多的帧意味着更多的处理，这通常会导致*停顿*和跳过（也称为*丢帧*或*jank）*。

如果您的监视器具有60Hz的刷新率，并且想要达到60 FPS，则大约需要16.7毫秒（`1000 / 60`）来执行动画代码以渲染每一帧。这是在提醒您，您需要牢记每次通过动画循环时尝试运行的代码量。

`requestAnimationFrame()`总是尝试尽可能接近这个神奇的60 FPS值。有时，这是不可能的-如果您的动画非常复杂并且在慢速的计算机上运行它，则帧速率会更低。在所有情况下， `requestAnimationFrame()`将始终利用可用的内容尽力而为。

### requestAnimationFrame（）与setInterval（）和setTimeout（）有何不同？



让我们再谈谈该`requestAnimationFrame()`方法与之前使用的其他方法的不同之处。从上面看我们的代码：

```js
function draw() {
   // Drawing code goes here
   requestAnimationFrame(draw);
}

draw();
```

现在让我们看看如何使用来做同样的事情`setInterval()`：

```js
function draw() {
   // Drawing code goes here
}

setInterval(draw, 17);
```

如前所述，您没有为指定时间间隔`requestAnimationFrame()`。它只是在当前条件下尽可能快，平稳地运行它。如果由于某种原因动画不在屏幕上，浏览器也不会浪费时间运行它。

`setInterval()`另一方面，*需要*指定一个间隔。通过公式*1000毫秒/ 60Hz*，我们得出了最终值17 ，然后将其四舍五入。四舍五入是一个好主意。如果四舍五入，浏览器可能会尝试以超过60 FPS的速度运行动画，并且无论如何它都不会影响动画的平滑度。正如我们之前所说，60Hz是标准刷新率。

### 包括时间戳



传递给该`requestAnimationFrame()`函数的实际回调也可以被赋予一个参数：*时间戳*值，表示自`requestAnimationFrame()`开始运行以来的时间。

这很有用，因为它使您可以在特定的时间以恒定的速度运行程序，而不管设备的运行速度有多快。您将使用的常规模式如下所示：

```js
let startTime = null;

function draw(timestamp) {
    if (!startTime) {
      startTime = timestamp;
    }

   currentTime = timestamp - startTime;

   // Do something based on current time

   requestAnimationFrame(draw);
}

draw();
```

### 浏览器支持



`requestAnimationFrame()`比`setInterval()`/ 更受最新浏览器支持`setTimeout()`。有趣的是，它在Internet Explorer 10及更高版本中可用。

因此，除非需要支持IE的较早版本，否则几乎没有理由不使用`requestAnimationFrame()`。

### 一个简单的例子



理论够了！让我们建立自己的个人 `requestAnimationFrame()`榜样。您将创建一个简单的“旋转动画”，即忙于连接服务器的应用程序中可能会显示的那种类型。

**注意**：在一个真实的示例中，您可能应该使用CSS动画来运行这种简单的动画。但是，这种示例对于演示`requestAnimationFrame()`用法非常有用，并且在执行更复杂的操作（例如更新每帧游戏的显示）时，您更有可能使用这种技术。

1. 抓取一个基本的HTML模板(）。

2. <div>在中<body>添加一个空元素，然后在其中添加一个↻字符。在此示例中，这是圆形箭头字符将充当我们的微调器。

3. 将以下CSS应用于HTML模板（以您喜欢的任何方式）。这会在页面上设置红色背景，将`<body>` 高度设置`100%`为的`<html>`高度，并将`<body>`内部的`<div>`水平和垂直居中。

   ```css
   html {
     background-color: white;
     height: 100%;
   }
   
   body {
     height: inherit;
     background-color: red;
     margin: 0;
     display: flex;
     justify-content: center;
     align-items: center;
   }
   
   div {
     display: inline-block;
     font-size: 10rem;
   }
   ```

4. <script>在</body>标签上方插入一个元素。

5. 将以下JavaScript插入您的``元素中。在这里，您将在``内部存储对常量的引用，将`rotateCount`变量设置为`0`，设置未初始化的变量（稍后将用于包含对`requestAnimationFrame()`调用的引用），并将`startTime`变量设置为`null`，稍后将用于存储的开始时间`requestAnimationFrame()`。

   ```js
   const spinner = document.querySelector('div');
   let rotateCount = 0;
   let startTime = null;
   let rAF;
   ```

6. 在前面的代码下面，插入一个`draw()`函数，该函数将用于包含我们的动画代码，其中包括`timestamp`参数：

   ```js
   function draw(timestamp) {
   
   }
   ```

7. 在内部`draw()`，添加以下行。他们将定义开始时间（如果尚未定义）（这只会在第一次循环迭代中发生），并将设置`rotateCount`为一个值以旋转微调器（当前时间戳记，取开始时间戳记，除以三，以此类推）不会太快）：

   ```js
     if (!startTime) {
      startTime = timestamp;
     }
   
     rotateCount = (timestamp - startTime) / 3;
   ```

8. 在其中的上一行下面`draw()`，添加以下代码块-这将检查的值`rotateCount`是否在`359`（例如`360`，一个完整的圆圈）之上。如果是这样，它会将值设置为其模数360（即，将值除以时剩下的余数`360`），从而使圆形动画可以不间断地以合理的低值继续进行。请注意，这并非绝对必要，但使用0–359度的值比使用诸如的值要容易`"128000 degrees"`。

   ```js
   if (rotateCount > 359) {
     rotateCount %= 360;
   }
   ```

9. 接下来，在上一个块下面添加以下行以实际旋转微调器：

   ```js
   spinner.style.transform = `rotate(${rotateCount}deg)`;
   ```

10. 在`draw()`函数的最底部，插入以下行。这是整个操作的关键-您正在将先前定义的变量设置为活动`requestAnimation()`调用，该调用将`draw()`函数作为其参数。这将启动动画，`draw()`并以接近60 FPS的速率不断运行该功能。

    ```js
    rAF = requestAnimationFrame(draw);
    ```



### 清除requestAnimationFrame（）调用



清除`requestAnimationFrame()`呼叫可以通过调用相应的`cancelAnimationFrame()`方法来完成。（请注意，函数名称以“ cancel”开头，而不是以“ set ...”方法为“ clear”开头。） 

只需将`requestAnimationFrame()`取消存储的调用返回的值传递给它，该值就存储在变量中`rAF`：

```js
cancelAnimationFrame(rAF);
```

### 主动学习：启动和停止微调器



在本练习中，我们希望您`cancelAnimationFrame()`通过前面的示例并对其进行更新来测试该方法，并添加一个事件侦听器以在单击页面上的任意位置时启动和停止微调器。

一些提示：

- 一个`click`事件处理程序可以被添加到大多数元素，包括文档`<body>`。如果您想最大化可点击区域，则将其放在元素上会更有意义-事件冒泡到其子元素。
- 您将需要添加一个跟踪变量，以检查微调框是否在旋转，如果清除动画帧，则清除动画帧，如果不是，则再次调用它。



### 限制requestAnimationFrame（）动画



的限制之一`requestAnimationFrame()`是您无法选择帧速率。在大多数情况下，这不是问题，因为通常您希望动画尽可能平滑地运行。但是，当您想创建老式的8位样式的动画时呢？

如果按`requestAnimationFrame()`，屏幕上显示的每个帧都显示一个不同的Sprite帧，Guybrush会使他的四肢移动得太快，并且动画看起来很荒谬。因此，此示例使用以下代码限制子画面循环其帧的速率：

```js
if (posX % 13 === 0) {
  if (sprite === 5) {
    sprite = 0;
  } else {
    sprite++;
  }
}
```

因此，代码仅每13个动画帧循环一次精灵。

...实际上，大约每6.5帧，因为我们每帧更新两个`posX`（字符在屏幕上的位置）：

```js
if (posX > width/2) {
  newStartPos = -( (width/2) + 102 );
  posX = Math.ceil(newStartPos / 13) * 13;
  console.log(posX);
} else {
  posX += 2;
}
```

这是计算如何更新每个动画帧中位置的代码。

用于限制动画的方法将取决于您的特定代码。例如，在较早的微调器示例中，您可以通过仅`rotateCount`在每个帧上增加一个而不是两个来使它看起来移动得更慢。

