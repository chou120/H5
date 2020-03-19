在本文中，我们简要回顾一下与同步JavaScript相关的问题，首次介绍你将遇到的一些不同的异步技术，并展示如何使用这些技术解决问题。

| 预备条件: | 基本的计算机素养，以及对JavaScript 基础知识的较好的理解。    |
| :-------- | ------------------------------------------------------------ |
| 目标:     | 熟悉什么是异步JavaScript，与同步JavaScript 的区别，以及使用场合。 |

## 同步JavaScript

要理解什么是**异步** JavaScript ，我们应该从确切理解**同步** JavaScript 开始。本节回顾我们在上一篇文章中看到的一些信息。

前面学的很多知识基本上都是同步的 — 运行代码，然后浏览器尽快返回结果。先看一个简单的例子

```js
const btn = document.querySelector('button');
btn.addEventListener('click', () => {
  alert('You clicked me!');

  let pElem = document.createElement('p');
  pElem.textContent = 'This is a newly-added paragraph.';
  document.body.appendChild(pElem);
});
```

这段代码, 一行一行的顺序执行：

1. 先取得一个在DOM里面的 `<button>`引用。
2. 点击按钮的时候，添加一个 `click` 事件监听器:
   1. `alert()` 消息出现。
   2. 一旦alert 结束，创建一个<p元素。
   3. 给它的文本内容赋值。
   4. 最后，把这个段落放进网页。

每一个操作在执行的时候，其他任何事情都没有发生 — 网页的渲染暂停. 因为前篇文章提到过 [JavaScript is single threaded](1/en-US/docs/Learn/JavaScript/Asynchronous/Concepts#JavaScript_is_single_threaded). 任何时候只能做一件事情, 只有一个主线程，其他的事情都阻塞了，直到前面的操作完成。

所以上面的例子，点击了按钮以后，段落不会创建，直到在alert消息框中点击ok，段落才会出现，你可以自己试试



**Note**: 这很重要请记住，`alert()`在演示阻塞效果的时候非常有用，但是在正式代码里面，它就是一个噩梦。

## 异步JavaScript

就前面提到的种种原因（比如，和阻塞相关）很多网页API特性使用异步代码，特别是从外部的设备上获取资源，譬如，从网络获取文件，访问数据库，从网络摄像头获得视频流，或者向VR头罩广播图像。

为什么使用异步代码这么难？看一个例子，当你从服务器获取一个图像，通常你不可能立马就得到，这需要时间，虽然现在的网络很快。这意味着下面的伪代码可能不能正常工作：

```js
var response = fetch('myImage.png');
var blob = response.blob();
// display your image blob in the UI somehow
```

因为你不知道下载图片会多久，所以第二行代码执行的时候可能报错（可能间歇的，也可能每次）因为图像还没有就绪。取代的方法就是，代码必须等到 `response` 返回才能继续往下执行。

在JavaScript代码中，你经常会遇到两种异步编程风格：老派callbacks，新派promise。下面就来分别介绍。

## 异步callbacks

异步callbacks 其实就是函数，只不过是作为参数传递给那些在后台执行的其他函数. 当那些后台运行的代码结束，就调用callbacks函数，通知你工作已经完成，或者其他有趣的事情发生了。使用callbacks 有一点老套，在一些老派但经常使用的API里面，你会经常看到这种风格。

举个例子，异步callback 就是[`addEventListener()`]( 1/docs/Web/API/EventTarget/addEventListener)第二个参数（前面的例子）：

```js
btn.addEventListener('click', () => {
  alert('You clicked me!');

  let pElem = document.createElement('p');
  pElem.textContent = 'This is a newly-added paragraph.';
  document.body.appendChild(pElem);
});
```

第一个参数是侦听的事件类型，第二个就是事件发生时调用的回调函数。.

当我们把回调函数作为一个参数传递给另一个函数时，仅仅是把回调函数定义作为参数传递过去 — 回调函数并没有立刻执行，回调函数会在包含它的函数的某个地方异步执行，包含函数负责在合适的时候执行回调函数。

你可以自己写一个容易的，包含回调函数的函数。来看另外一个例子，用 [`XMLHttpRequest` API ([运行它]( 1/learning-area/javascript/asynchronous/introducing/xhr-async-callback.html), and [源代码](https://github.com/mdn/learning-area/blob/master/javascript/asynchronous/introducing/xhr-async-callback.html)) 加载资源：

```js
function loadAsset(url, type, callback) {
  let xhr = new XMLHttpRequest();
  xhr.open('GET', url);
  xhr.responseType = type;

  xhr.onload = function() {
    callback(xhr.response);
  };

  xhr.send();
}

function displayImage(blob) {
  let objectURL = URL.createObjectURL(blob);

  let image = document.createElement('img');
  image.src = objectURL;
  document.body.appendChild(image);
}

loadAsset('coffee.jpg', 'blob', displayImage);
```

创建 `displayImage()` 函数，简单的把blob传递给它，生成objectURL，然后再生成一个image元素，把objectURL作为image的源地址，最后显示这张图片。 然后，我们创建 `loadAsset()` 函数，把URL，type，和回调函数同时都作为参数。函数用 `XMLHttpRequest` (通常缩写 "XHR") 获取给定URL的资源，在获得资源响应后再把响应作为参数传递给回调函数去处理。 (使用 `onload` 事件处理) ，有点烧脑，是不是？！

回调函数用途广泛 — 他们不仅仅可以用来控制函数的执行顺序和函数之间的数据传递，还可以根据环境的不同，将数据传递给不同的函数，所以对下载好的资源，你可以采用不同的操作来处理，譬如 `processJSON()`, `displayText()`, 等等。

请注意，不是所有的回调函数都是异步的 — 有一些是同步的。一个例子就是使用 [`Array.prototype.forEach()`]( 1/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) 来遍历数组 

```js
const gods = ['Apollo', 'Artemis', 'Ares', 'Zeus'];

gods.forEach(function (eachName, index){
  console.log(index + '. ' + eachName);
});
```

在这个例子中，我们遍历一个希腊神的数组，并在控制台中打印索引和值。`forEach()` 需要的参数是一个回调函数，回调函数本身带有两个参数，数组元素和索引值。它无需等待任何事情，立即运行。

## Promises

Promises 是新派的异步代码，现代的web APIs经常用到。 `fetch()` API就是一个很好的例子, 它基本上就是一个现代版的，更高效的 [`XMLHttpRequest`]( 1/docs/Web/API/XMLHttpRequest)。

```js
fetch('products.json').then(function(response) {
  return response.json();
}).then(function(json) {
  products = json;
  initialize();
}).catch(function(err) {
  console.log('Fetch problem: ' + err.message);
});
```



这里`fetch()` 只需要一个参数— 资源的网络 URL — 返回一个 [promise. promise 是表示异步操作完成或失败的对象。可以说，它代表了一种中间状态。 本质上，这是浏览器说“我保证尽快给您答复”的方式，因此得名“promise”。

这个概念需要练习来适应;它感觉有点像运行中的[薛定谔猫](https://zh.wikipedia.org/wiki/薛定谔猫)。这两种可能的结果都还没有发生，因此fetch操作目前正在等待浏览器试图在将来某个时候完成该操作的结果。然后我们有三个代码块链接到fetch()的末尾：

- 两个 `then()` 块。两者都包含一个回调函数，如果前一个操作成功，该函数将运行，并且每个回调都接收前一个成功操作的结果作为输入，因此您可以继续对它执行其他操作。每个 `.then()`块返回另一个promise，这意味着可以将多个`.then()`块链接到另一个块上，这样就可以依次执行多个异步操作。
- 如果其中任何一个`then()`块失败，则在末尾运行`catch()`块——与同步`try...catch`类似，`catch()`提供了一个错误对象，可用来报告发生的错误类型。但是请注意，同步`try...catch`不能与promise一起工作，尽管它可以与[async/await一起工作，稍后您将了解到这一点。

**Note**: 在本模块稍后的部分中，你将学习更多关于promise的内容，所以如果你还没有完全理解这些promise，请不要担心。

### 事件队列



像promise这样的异步操作被放入事件队列中，事件队列在主线程完成处理后运行，这样它们就不会阻止后续JavaScript代码的运行。排队操作将尽快完成，然后将结果返回到JavaScript环境。

### Promises 对比 callbacks



promises与旧式callbacks有一些相似之处。它们本质上是一个返回的对象，您可以将回调函数附加到该对象上，而不必将回调作为参数传递给另一个函数。

然而，`Promise`是专门为异步操作而设计的，与旧式回调相比具有许多优点:

- 您可以使用多个then()操作将多个异步操作链接在一起，并将其中一个操作的结果作为输入传递给下一个操作。这种链接方式对回调来说要难得多，会使回调以混乱的“末日金字塔”告终。
- `Promise`总是严格按照它们放置在事件队列中的顺序调用。
- 错误处理要好得多——所有的错误都由块末尾的一个.catch()块处理，而不是在“金字塔”的每一层单独处理。

## 异步代码的本质

让我们研究一个示例，它进一步说明了异步代码的本质，展示了当我们不完全了解代码执行顺序以及将异步代码视为同步代码时可能发生的问题。下面的示例与我们之前看到的非常相似。一个不同之处在于，我们包含了许多[`console.log()`]( 1/docs/Web/API/Console/log)语句，以展示代码将在其中执行的顺序。

```js
console.log ('Starting');
let image;

fetch('coffee.jpg').then((response) => {
  console.log('It worked :)')
  return response.blob();
}).then((myBlob) => {
  let objectURL = URL.createObjectURL(myBlob);
  image = document.createElement('img');
  image.src = objectURL;
  document.body.appendChild(image);
}).catch((error) => {
  console.log('There has been a problem with your fetch operation: ' + error.message);
});

console.log ('All done!');
```

浏览器将会执行代码，看见第一个`console.log()` 输出(`Starting`) ，然后创建`image` 变量。

然后，它将移动到下一行并开始执行`fetch()`块，但是，因为`fetch()`是异步执行的，没有阻塞，所以在`promise`相关代码之后程序继续执行，从而到达最后的`console.log()`语句(`All done`!)并将其输出到控制台。

只有当`fetch()` 块完成运行返回结果给`.then()` ，我们才最后看到第二个`console.log()` 消息 (`It worked ;)`) . 所以 这些消息 可能以 和你预期不同的顺序出现：

- Starting
- All done!
- It worked :)

如果你感到疑惑，考虑下面这个小例子：

```js
console.log("registering click handler");

button.addEventListener('click', () => {
  console.log("get click");
});

console.log("all done");
```

这在行为上非常相似——第一个和第三个`console.log()`消息将立即显示，但是第二个消息将被阻塞，直到有人单击鼠标按钮。前面的示例以相同的方式工作，只是在这种情况下，第二个消息在`promise`链上被阻塞，直到获取资源后再显示在屏幕上，而不是单击。

