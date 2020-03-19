async functions和`await` 关键字是最近添加的JavaScript语言，这是所谓的ECMAScript 2017 JavaScript版的一部分（参见[ECMAScript Next support in Mozilla）。这些功能基本上充当promises语法上的甜头，使得异步代码更易于编写和后续阅读。它们使异步代码看起来更像是旧式同步代码，所以它们非常值得学习。本文为您提供了您需要了解的内容。

| 预备条件: | 基本的计算机知识，对JavaScript基础知识的合理理解，对一般异步代码和promise的理解。 |
| :-------- | ------------------------------------------------------------ |
| 目标:     | 理解并使用Promises                                           |

## async/await的基本要素

以下是在代码中使用async / await的两个部分。

### async关键字



首先，我们使用`async`关键字，您可以将它放在函数声明之前，将其转换为[async function。异步函数是一个知道怎样预期 await 关键字可用于调用异步代码可能性的函数。

尝试在浏览器的JS控制台中键入以下行：

```js
function hello() { return "Hello" };
hello();
```

该函数返回“Hello” ––没什么特别的，对吧？

但是，如果我们将其变成异步函数呢？请尝试以下方法：

```js
async function hello() { return "Hello" };
hello();
```

额。。现在调用该函数会返回一个promise。这是异步函数的特征之一 ––它将任何函数转换为promise。

你也可以创建一个异步函数表达式（参见[async function expression），如下所示:

```js
let hello = async function() { return "Hello" };
hello();
```

你可以使用箭头函数：

```js
let hello = async () => { return "Hello" };
```

这些都基本上是一样的。

要实际使用promise实现时返回的值，因为它返回了一个promise，我们可以使用`.then（）`块：

```js
hello().then((value) => console.log(value))
```

甚至只是简写如

```js
hello().then(console.log)
```

就像我们在上一篇文章中看到的那样

因此，将`async`关键字添加到函数中以告诉它们返回promise而不是直接返回值。此外，这使同步函数可以避免运行支持使用`await`时带来的任何潜在开销。通过仅在函数声明为异步时添加必要的处理，JavaScript引擎可以为您优化您的程序。爽！

### await关键字



将它与[await关键字结合使用时，异步函数的真正优势就变得明显了。这可以放在任何基于异步声明的函数之前，暂停代码在该行上，直到promise完成，然后返回结果值。与此同时，可能正在等待执行机会的其他代码也会这样做。

您可以在调用任何返回Promise的函数时使用`await`，包括Web API函数。

这是一个简单的示例：

```js
async function hello() {
  return greeting = await Promise.resolve("Hello");
};

hello().then(alert);
```

当然，上面的示例并不是很有用，尽管它确实用于说明语法。让我们继续前进，看一个真实示例。

##  使用async/await重写promise代码

让我们回顾一下我们在上一篇文章中看到的一个简单的获取示例：

```js
fetch('coffee.jpg')
.then(response => response.blob())
.then(myBlob => {
  let objectURL = URL.createObjectURL(myBlob);
  let image = document.createElement('img');
  image.src = objectURL;
  document.body.appendChild(image);
})
.catch(e => {
  console.log('There has been a problem with your fetch operation: ' + e.message);
});
```

到现在为止，你应该对promises及其工作方式有一个恰当的理解，让我们把它转换为使用async / await来看看它做起来有多么简单：

```js
async function myFetch() {
  let response = await fetch('coffee.jpg');
  let myBlob = await response.blob();

  let objectURL = URL.createObjectURL(myBlob);
  let image = document.createElement('img');
  image.src = objectURL;
  document.body.appendChild(image);
}

myFetch();
```

它使代码更简单，更容易理解 — 不会再有到处都是`.then()`代码块！

由于`async`关键字将函数转换为promise，因此您可以重构代码以使用`promises`和`await`的混合方法，将函数的后半部分放入新块中以使其更灵活：

```js
async function myFetch() {
  let response = await fetch('coffee.jpg');
  return await response.blob();
}

myFetch().then((blob) => {
  let objectURL = URL.createObjectURL(blob);
  let image = document.createElement('img');
  image.src = objectURL;
  document.body.appendChild(image);
});
```



### 它到底是如何工作的？



您会注意到我们已经将代码封装在函数中，并且我们在`function`关键字之前包含了`async`关键字。这是必要的 –– 您必须创建一个异步函数来定义一个代码块，您将在其中运行异步代码;`await`只能在异步函数内部工作。

**注意:** 值得一提的是，在一个具有引人注目的背景颜色的盒子中：await仅适用于异步函数。

在`myFetch（）`函数定义中，您可以看到代码与先前的promise版本非常相似，但存在一些差异。不需要链接`.then()`代码块到每个promise-based方法的结尾，你只需要在方法调用前添加的await关键字，然后把结果赋给变量。 await关键字使JavaScript运行时暂停此行上的代码，允许其他代码在此期间执行，直到异步函数调用返回其结果。一旦完成，您的代码将继续从下一行开始执行。例如：

```js
let response = await fetch('coffee.jpg');
```

当响应变得可用时，由实现的（fullfilled）`fetch（）`promise返回的response被分配给`response`变量，然后解析器在此行上暂停，直到它再次出现。一旦响应可用，解析器就会移动到下一行，从而创建一个`Blob`。此行还调用基于异步promise的方法，因此我们也在此处使用`await`。当操作结果返回时，我们将它从`myFetch（）`函数中返回。

这意味着当我们调用`myFetch（）`函数时，它会返回一个promise，因此我们可以将`.then（）`链接到它的末尾，在其中我们处理显示在屏幕上的`blob`。

你可能已经在考虑“这真的很酷！”，你是对的 ––用更少的.`then（）`块来封装代码，它大多只是看起来像同步代码，所以它非常直观。

### 添加错误处理



如果你想添加错误处理，你有几个选择。

您可以使用`async / await`同步`try...catch`结构。此示例扩展了我们上面显示的代码的第一个版本：

```js
async function myFetch() {
  try {
    let response = await fetch('coffee.jpg');
    let myBlob = await response.blob();

    let objectURL = URL.createObjectURL(myBlob);
    let image = document.createElement('img');
    image.src = objectURL;
    document.body.appendChild(image);
  } catch(e) {
    console.log(e);
  }
}

myFetch();
```

`catch（）{}`代码块传递一个错误对象，我们称之为`e`;我们现在可以将其记录到控制台，它将向我们提供详细的错误消息，显示错误被抛出的代码中的位置。

如果你想使用我们上面展示的代码的第二个（重构）版本，你最好继续混合方法并将`.catch（）`块链接到`.then（）`调用的末尾，就像这样：

```js
async function myFetch() {
  let response = await fetch('coffee.jpg');
  return await response.blob();
}

myFetch().then((blob) => {
  let objectURL = URL.createObjectURL(blob);
  let image = document.createElement('img');
  image.src = objectURL;
  document.body.appendChild(image);
})
.catch((e) =>
  console.log(e)
);
```

这是因为`.catch（）`块将捕获异步函数调用和promise链中发生的错误。如果您在此处使用了`try / catch`代码块，则在调用`myFetch（）`函数时，您仍可能会收到未处理的错误。

## 等待Promise.all()

`async / await`建立在[promises之上，因此它与promises提供的所有功能兼容。这包括`Promise.all()` –– 您可以非常高兴地等待`Promise.all（）`调用，以一种看起来像简单同步代码的方式将所有结果返回到变量中。



```js
async function fetchAndDecode(url, type) {
  let response = await fetch(url);

  let content;

  if(type === 'blob') {
    content = await response.blob();
  } else if(type === 'text') {
    content = await response.text();
  }

  return content;
}

async function displayContent() {
  let coffee = fetchAndDecode('coffee.jpg', 'blob');
  let tea = fetchAndDecode('tea.jpg', 'blob');
  let description = fetchAndDecode('description.txt', 'text');

  let values = await Promise.all([coffee, tea, description]);

  let objectURL1 = URL.createObjectURL(values[0]);
  let objectURL2 = URL.createObjectURL(values[1]);
  let descText = values[2];

  let image1 = document.createElement('img');
  let image2 = document.createElement('img');
  image1.src = objectURL1;
  image2.src = objectURL2;
  document.body.appendChild(image1);
  document.body.appendChild(image2);

  let para = document.createElement('p');
  para.textContent = descText;
  document.body.appendChild(para);
}

displayContent()
.catch((e) =>
  console.log(e)
);
```

您将看到`fetchAndDecode（）`函数已经轻松转换为异步函数，只需进行一些更改。请查看`Promise.all（）`行：

```js
let values = await Promise.all([coffee, tea, description]);
```

在这里通过使用await，当它们全部可用时，我们能够获得返回到`values`数组中的三个promise的所有结果，其方式看起来非常像同步代码。我们必须将所有代码封装在一个新的异步函数`displayContent（）`中，并且我们没有通过很多行来减少代码，但能够将大部分代码移除`.then（）` 代码块提供了一个很好的，有用的简化，让我们有了一个更易读的程序。

对于错误处理，我们在`displayContent（）`调用中包含了一个`.catch（）`代码块;这将处理两个函数中出现的错误。

## async/await的缺陷

Async / await对于了解非常有用，但还有一些缺点需要考虑。

Async / await使您的代码看起来是同步的，并且在某种程度上它使它的行为更加同步。 `await`关键字阻止执行所有代码，直到promise完成，就像执行同步操作一样。它允许其他任务在此期间继续运行，但您自己的代码被阻止。

这意味着您的代码可能会因为大量等待的promises相继发生而变慢。每个`await`将等待前一个完成，而实际上你想要的是promises同时开始处理（就像我们没有使用async / await时那样）。

有一种模式可以缓解这个问题 ––通过关闭所有promise进程将`Promise`对象存储在变量中，然后等待触发它们。让我们看一些证明这个概念的例子。

我们有两个可用的例子它们都以自定义promise函数开始，该函数使用`setTimeout（）`调用伪造异步进程：

```js
function timeoutPromise(interval) {
  return new Promise((resolve, reject) => {
    setTimeout(function(){
      resolve("done");
    }, interval);
  });
};
```

然后每个包含一个`timeTest（）`异步函数，等待三个`timeoutPromise（）`调用：

```js
async function timeTest() {
  ...
}
```

每一个都以记录开始时间结束，查看`timeTest（）`promise需要多长时间才能完成，然后记录结束时间并报告操作总共需要多长时间：

```js
let startTime = Date.now();
timeTest().then(() => {
  let finishTime = Date.now();
  let timeTaken = finishTime - startTime;
  alert("Time taken in milliseconds: " + timeTaken);
})
```

`timeTest（）`函数在每种情况下都不同。

```js
async function timeTest() {
  await timeoutPromise(3000);
  await timeoutPromise(3000);
  await timeoutPromise(3000);
}
```

在这里，我们直接等待所有三个timeoutPromise（）调用，使每个调用3秒钟。后续的每一个都被迫等到最后一个完成 - 如果你运行第一个例子，你会看到警报框报告的总运行时间大约为9秒。

```js
async function timeTest() {
  const timeoutPromise1 = timeoutPromise(3000);
  const timeoutPromise2 = timeoutPromise(3000);
  const timeoutPromise3 = timeoutPromise(3000);

  await timeoutPromise1;
  await timeoutPromise2;
  await timeoutPromise3;
}
```

在这里，我们将三个Promise对象存储在变量中，这样可以同时启动它们关联的进程。

接下来，我们等待他们的结果 - 因为promise都在基本上同时开始处理，promise将同时完成;当您运行第二个示例时，您将看到警报框报告总运行时间超过3秒！

您必须仔细测试您的代码，并在性能开始受损时牢记这一点。

另一个小小的不便是你必须将期待已久的promise封装在异步函数中。

## Async/await 的类方法

最后值得一提的是，我们可以在类/对象方法前面添加`async`，以使它们返回promises，并`await`它们内部的promises。

```js
class Person {
  constructor(first, last, age, gender, interests) {
    this.name = {
      first,
      last
    };
    this.age = age;
    this.gender = gender;
    this.interests = interests;
  }

  async greeting() {
    return await Promise.resolve(`Hi! I'm ${this.name.first}`);
  };

  farewell() {
    console.log(`${this.name.first} has left the building. Bye for now!`);
  };
}

let han = new Person('Han', 'Solo', 25, 'male', ['Smuggling']);
```

第一类方法现在可以使用如下：

```js
han.greeting().then(console.log);
```

 