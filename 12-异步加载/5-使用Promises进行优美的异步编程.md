**承诺**是JavaScript语言的一项相对较新的功能，可让您将进一步的操作推迟到上一个操作完成或对它的失败做出响应之前。这对于设置一系列异步操作以使其正常工作很有用。本文向您展示了承诺如何工作，如何将其与Web API一起使用以及如何编写自己的承诺。

| 先决条件： | 基本的计算机知识，对JavaScript基础的合理理解。 |
| :--------- | ---------------------------------------------- |
| 目的：     | 了解承诺以及如何使用它们。                     |

## 什么是Promise？

在本课程的第一篇文章中，我们简要地介绍了[Promises，但在这里我们将更深入地探讨它们。

从本质上讲，Promise是表示操作的中间状态的对象-实际上，是将来会在某个时候返回某种结果的*保证*。不能保证确切地知道何时该操作将完成并返回结果，但是*可以*保证当结果可用或承诺失败时，将执行您提供的代码，以便用成功的结果，或者很好地处理失败案例。

一般情况下，你是不感兴趣的时间异步操作将返回其结果量（当然，除非它需要*远远*太长！），更感兴趣的是能够响应它返回，只要那是。当然，它不会阻塞其余的代码执行也很好。

与诺言最常见的约定之一就是返回诺言的Web API。让我们考虑一个假设的视频聊天应用程序。该应用程序具有一个包含用户好友列表的窗口，单击用户旁边的按钮可发起对该用户的视频通话。

该按钮的处理程序将调用[`getUserMedia()`以便访问用户的摄像头和麦克风。由于`getUserMedia()`必须确保用户有权使用这些设备*并*询问用户使用哪个麦克风和使用哪个摄像头（或是否为仅语音呼叫，以及其他可能的选择），因此它可以阻止直到所有这些决定中，还有摄像头和麦克风已投入使用。此外，用户可能不会立即响应这些权限请求。这可能会花费很长时间。

由于`getUserMedia()`是从浏览器的主线程进行的调用，因此整个浏览器将被阻止，直到`getUserMedia()`返回为止！显然，这不是一个可以接受的选择。没有承诺，浏览器中的所有内容将变得无法使用，直到用户决定如何处理摄像头和麦克风。因此，与其等待用户，不如启用所选设备，并直接返回[`MediaStream`从所选源创建的流的，而是在可用后立即`getUserMedia()`返回。[`promise`[`MediaStream` 

视频聊天应用程序将使用的代码如下所示：

```js
function handleCallButton(evt) {
  setStatusMessage("Calling...");
  navigator.mediaDevices.getUserMedia({video: true, audio: true})
    .then(chatStream => {
      selfViewElem.srcObject = chatStream;
      chatStream.getTracks().forEach(track => myPeerConnection.addTrack(track, chatStream));
      setStatusMessage("Connected");
    }).catch(err => {
      setStatusMessage("Failed to connect");
    });
}
```

此功能通过使用一个名为`setStatusMessage()`“ Calling ...”的消息来更新状态显示的函数来开始，该消息表示正在尝试进行呼叫。然后`getUserMedia()`，它调用，要求同时包含视频和音频轨道的流，然后获取该流，然后设置视频元素以将来自摄像机的流显示为“自视图”，然后获取该流的每个轨道并将它们添加到[WebRTC， [`RTCPeerConnection`代表与另一个用户的连接。之后，状态显示更新为“已连接”。

如果`getUserMedia()`失败，则该`catch`块运行。这用于`setStatusMessage()`更新状态框以指示发生了错误。

这里重要的是`getUserMedia()`，即使尚未获取摄像机流，呼叫也几乎立即返回。即使该`handleCallButton()`函数已经返回到调用它的代码，在`getUserMedia()`完成工作后，它也会调用您提供的处理程序。只要应用程序不认为流媒体已经开始，它就可以继续运行。

**注意：**如果您有兴趣，可以在[信号和视频通话文章中了解有关此高级主题的更多信息。在该示例中使用了与此类似但更完整的代码。

## 回调的麻烦

要完全理解诺言为什么是一件好事，它有助于回顾一下老式的回调并理解它们为什么有问题。

让我们谈谈订购披萨作为类比。为了使订单成功，您必须采取某些步骤，尝试无序执行或按顺序执行，但在每个上一步完成之前，这实际上没有任何意义：

1. 您选择想要的浇头。如果您犹豫不决，这可能需要一段时间，如果您不能下定决心，或者决定要咖喱，则可能会失败。
2. 然后，您下订单。这可能需要一段时间才能归还比萨饼，如果餐厅没有所需的食材来烹饪，可能会失败。
3. 然后，您收集比萨饼并进餐。例如，如果您忘记了钱包，因此无法支付披萨，则可能会失败！

使用旧式[回调](1/en-US/docs/Learn/JavaScript/Asynchronous/Introducing#Callbacks)，上述功能的伪代码表示可能类似于以下内容：

```js
chooseToppings(function(toppings) {
  placeOrder(toppings, function(order) {
    collectOrder(order, function(pizza) {
      eatPizza(pizza);
    }, failureCallback);
  }, failureCallback);
}, failureCallback);
```

这是凌乱且难以阅读的（通常称为“回调地狱”），需要`failureCallback()`多次调用（每个嵌套函数一次），此外还有其他问题。

### Promise改善



承诺使上述情况更加容易编写，解析和运行。如果我们使用异步Promise来表示上述伪代码，则最终会得到如下所示：

```js
chooseToppings()
.then(function(toppings) {
  return placeOrder(toppings);
})
.then(function(order) {
  return collectOrder(order);
})
.then(function(pizza) {
  eatPizza(pizza);
})
.catch(failureCallback);
```

这样做好多了–可以很容易地看到发生了什么，我们只需要一个`.catch()`块即可处理所有错误，而不会阻塞主线程（因此我们可以继续玩视频游戏，而我们等比萨饼就可以了。准备好收集），并且确保每个操作在运行之前都等待先前的操作完成。我们能够以这种方式将多个异步动作链接在一起，因为每个`.then()`块都返回一个新的Promise，该诺言在该`.then()`块完成运行时就解决。聪明吧？

使用箭头功能，您可以进一步简化代码：

```js
chooseToppings()
.then(toppings =>
  placeOrder(toppings)
)
.then(order =>
  collectOrder(order)
)
.then(pizza =>
  eatPizza(pizza)
)
.catch(failureCallback);
```

甚至这个：

```js
chooseToppings()
.then(toppings => placeOrder(toppings))
.then(order => collectOrder(order))
.then(pizza => eatPizza(pizza))
.catch(failureCallback);
```

之所以`() => x`有效，是因为带箭头功能是的有效简写`() => { return x; }`。

您甚至可以执行此操作，因为这些函数只是直接传递其参数，因此不需要额外的函数层：

```js
chooseToppings().then(placeOrder).then(collectOrder).then(eatPizza).catch(failureCallback);
```

但是，这不太容易阅读，如果您的代码块比我们在此显示的要复杂，则此语法可能不可用。

**注意**：您可以使用`async`/ `await`语法进行进一步的改进，我们将在下一篇文章中进行深入探讨。

从本质上讲，promise与事件侦听器相似，但有一些区别：

- 一个承诺只能成功或失败一次。操作完成后，它不能成功或失败两次，也不能从成功切换为失败，反之亦然。
- 如果某个诺言成功或失败，并且您以后添加了成功/失败回调，则即使事件较早发生，也会调用正确的回调。

## 解释基本的Promise语法：一个真实的例子

理解这一点很重要，因为大多数现代的Web API都将它们用于执行可能冗长的任务的功能。要使用现代网络技术，您需要使用promise。在本章的后面，我们将研究如何编写自己的Promise，但是现在，我们将研究Web API中遇到的一些简单示例。

在第一个示例中，我们将使用该`fetch()`方法从Web上获取图像，该[`blob()`方法将获取响应的原始内容转换为[`Blob`对象，然后在`<img>`元素内部显示该Blob 。这与我们在[本系列](1/en-US/docs/Learn/JavaScript/Asynchronous/Introducing#Asynchronous_JavaScript)的[第一篇文章中](1/en-US/docs/Learn/JavaScript/Asynchronous/Introducing#Asynchronous_JavaScript)看到的示例非常相似，但是当您构建自己的基于Promise的代码时，我们会做些不同的事情。

**注意**：如果仅从文件直接运行（即通过`file://`URL），则以下示例将不起作用。您需要通过[本地测试服务器运行它，或使用在线解决方案，例如[Glitch](https://glitch.com/)或[GitHub页面。

1. 首先，下载我们的[简单HTML模板](https://github.com/mdn/learning-area/blob/master/html/introduction-to-html/getting-started/index.html)和我们将获取的[示例图像文件]()

2. <script>在HTML的底部添加一个元素<body>。

3. 在您的`<script>`元素内，添加以下行：

   ```js
   let promise = fetch('coffee.jpg');
   ```

   这将调用该`fetch()`方法，并将要传递给网络的图像URL作为参数传递给该方法。这也可以将options对象作为可选的第二个参数，但是我们现在仅使用最简单的版本。我们将存储在`fetch()`称为的变量内部返回的Promise对象`promise`。就像我们之前说过的那样，此对象表示一个初始状态既不是成功也不是失败的中间状态-此状态下的诺言的正式术语**尚待确认**。

4. 为了响应成功完成的操作（在这种情况下，当[`Response`返回a时），我们调用`.then()`promise对象的方法。内部的回调`.then()`块（简称**执行人**），只有当承诺调用成功完成，并返回运行[`Response`中的承诺发言，当它已经-对象**实现**。将返回的[`Response`对象作为参数传递给它。

   **注意**：`.then()`块的工作方式类似于使用来向对象添加事件侦听器时`AddEventListener()`。直到事件发生时（诺言履行时），它才会运行。最明显的区别是.then（）每次使用仅会运行一次，而事件侦听器可以多次调用。

   我们立即`blob()`在该响应上运行该方法，以确保完整下载了响应主体，并在可用时将其转换为`Blob`可以处理的对象。这样返回的结果如下：

   ```js
   response => response.blob()
   ```

   这是简写

   ```js
   function(response) {
     return response.blob();
   }
   ```

   好，现在有足够的解释。在JavaScript的第一行下方添加以下行。

   ```js
   let promise2 = promise.then(response => response.blob());
   ```

5. 每次致电`.then()`都会创建一个新的承诺。这非常有用；因为该`blob()`方法还返回了一个Promise，所以我们可以`Blob`通过调用`.then()`第二个Promise的方法来处理它在实现时返回的对象。因为我们想要对blob进行一些操作，而不是对它单独运行一个简单的方法并返回结果，所以这次我们需要将函数体用大括号括起来（否则会引发错误）。

   在代码末尾添加以下内容：

   ```js
   let promise3 = promise2.then(myBlob => {
   
   })
   ```

6. 现在让我们填写执行程序函数的主体。在花括号内添加以下行：

   ```js
   let objectURL = URL.createObjectURL(myBlob);
   let image = document.createElement('img');
   image.src = objectURL;
   document.body.appendChild(image);
   ```

   在这里，我们运行[`URL.createObjectURL()`方法，将其作为参数传递`Blob`给第二个Promise满足时返回。这将返回指向该对象的URL。然后，我们创建一个`<img>`元素，将其`src`属性设置为等于对象URL并将其附加到DOM，这样图像就会显示在页面上！

如果保存刚创建的HTML文件并将其加载到浏览器中，则会看到图像按预期显示在页面中。干得好！

**注意**：您可能会注意到这些示例有些人为设计。您可以删除整个`fetch()`和`blob()`链，只需创建一个`<img>`元素并将其`src`属性值设置为图像文件的URL即可`coffee.jpg`。但是，我们确实选择了此示例，因为它以一种很好的简单方式展示了诺言，而不是出于其在现实世界中的适用性。

### 应对失败



缺少某些内容-当前，如果其中一个诺言失败（**拒绝**，在诺言中），则没有任何东西可以显式处理错误。我们可以通过运行`.catch()`上一个promise 的方法来添加错误处理。立即添加：

```js
let errorCase = promise3.catch(e => {
  console.log('There has been a problem with your fetch operation: ' + e.message);
});
```

要查看实际效果，请尝试将图片的URL拼写错误并重新加载页面。该错误将在浏览器开发人员工具的控制台中报告。

如果您根本不去考虑完全包含该`.catch()`块，那么这并没有多做，而是考虑一下—这使我们能够按照自己想要的方式控制错误处理。在实际应用中，您的代码`.catch()`块可以重试获取图像，或显示默认图像，或提示用户提供其他图像URL，或其他。

### 将块链接在一起



这是写出来的很长的方法。我们故意这样做是为了帮助您清楚地了解正在发生的事情。如本文前面所述，您可以将`.then()`块（以及`.catch()`块）链接在一起。

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

请记住，由已兑现的承诺返回的值将成为传递给下一个`.then()`块的执行程序函数的参数。

**注意**：Promise中的`.then()`/ `.catch()`块基本上与`try...catch`同步代码中的块异步等效。请记住，同步`try...catch`在异步代码中不起作用。

## 无极术语回顾

上一节将介绍很多内容，因此让我们快速回顾一下，为您提供一个[简短的指南，您可以将其添加为书签](1/en-US/docs/Learn/JavaScript/Asynchronous/Promises#Promise_terminology_recap)并在将来用于刷新内存。您还应该再次检查以上部分，以确保这些概念仍然有效。

1. 创建承诺后，它既不会处于成功状态，也不会处于失败状态。据说还有待**处理**。

2. 当诺言返回时，据说已经解决了

   1. 一个成功的解析承诺被认为是**满足**。它返回一个值，可以通过将一个`.then()`块链接到promise链的末尾来访问它。`.then()`块内的执行程序函数将包含promise的返回值。
2. 不能解决的诺言据说被**拒绝了**。它返回一个**原因**，一条错误消息，说明为什么诺言被拒绝。可以通过将一个`.catch()`块链接到Promise链的末尾来访问此原因。

## 运行代码以实现多个承诺

上面的示例向我们展示了使用promise的一些实际基础。现在，让我们看一些更高级的功能。首先，将链接过程一个接一个地进行是可以的，但是如果您只想在全部承诺*都*完成后才运行某些代码，该怎么办？

您可以使用巧妙命名的`Promise.all()`静态方法来执行此操作。这将一个promise数组作为输入参数，并返回一个新`Promise`对象，该对象仅在且仅当阵列fulfil中的*所有* promise 满足时才会实现。看起来像这样：

```js
Promise.all([a, b, c]).then(values => {
  ...
});
```

如果它们全部实现，则将向链接`.then()`块的执行程序函数传递一个包含所有这些结果作为参数的数组。如果有任何承诺被`Promise.all()`拒绝，整个区块将被拒绝。

这可能非常有用。想象一下，我们正在获取信息以动态地在页面上使用内容填充UI功能。在许多情况下，接收所有数据然后才显示完整的内容而不是显示部分信息是有意义的。

让我们构建另一个示例来展示这一点。

1. 下载我们[页面模板](https://github.com/mdn/learning-area/blob/master/html/introduction-to-html/getting-started/index.html)的新副本，然后在结束标记之前再次放置一个元素。

2. 下载我们的源文件（[coffee.jpg]()，[tea.jpg]()和[description.txt]()，或者随时替换您自己的文件。

3. 在脚本中，我们将首先定义一个函数，该函数返回要发送给的promise `Promise.all()`。如果我们只是想`Promise.all()`响应三个`fetch()`操作完成而运行该块，这将很容易。我们可以做这样的事情：

   ```js
   let a = fetch(url1);
   let b = fetch(url2);
   let c = fetch(url3);
   
   Promise.all([a, b, c]).then(values => {
     ...
   });
   ```

   兑现承诺后，`values`传递到实现处理程序中的`Response`对象将包含三个对象，每个`fetch()`完成的操作一个。

   但是，我们不想这样做。我们的代码不在乎`fetch()`操作何时完成。相反，我们想要的是加载的数据。这意味着我们要在`Promise.all()`返回代表图像的可用blob和可用文本字符串时运行该块。我们可以编写一个执行此操作的函数；在您的`<script>`元素内添加以下内容：

   ```js
   function fetchAndDecode(url, type) {
     return fetch(url).then(response => {
       if (type === 'blob') {
         return response.blob();
       } else if (type === 'text') {
         return response.text();
       }
     })
     .catch(e => {
       console.log('There has been a problem with your fetch operation: ' + e.message);
     });
   }
   ```

   这看起来有点复杂，所以让我们逐步进行操作：

   1. 首先，我们定义函数，并向其传递一个URL和一个字符串，该字符串代表要获取的资源类型。
   2. 在函数体内，我们具有与第一个示例中相似的结构–我们调用该`fetch()`函数以获取指定URL处的资源，然后将其链接到另一个返回已解码（或“读取”）响应正文的Promise中。 。这始终`blob()`是前面示例中的方法。
   3. 但是，这里有两点不同：
      - 首先，我们返回的第二个承诺根据`type`值是不同的。在executor函数内部，我们包含一个简单的`if ... else if`语句，该语句根据需要解码的文件类型返回不同的promise（在这种情况下，我们可以选择`blob`or `text`，但是可以很容易地扩展它以处理其他文件类型）。
      - 其次，我们`return`在`fetch()`呼叫之前添加了关键字。这样的效果是运行整个链，然后运行最终结果（即由`blob()`或返回的promise `text()`）作为我们刚刚定义的函数的返回值。实际上，这些`return`语句将结果传递回链顶部。
   4. 在代码块的末尾，我们链接一个`.catch()`调用，以处理可能发生在数组中传递给的任何promise的错误情况`.all()`。如果有任何承诺被拒绝，则catch块将让您知道哪个有问题。该`.all()`块（请参见下文）仍将实现，但不会显示有问题的资源。如果`.all`要拒绝，则必须将`.catch()`块链接到该块的末尾。

   函数体内的代码是异步的并且基于Promise，因此，实际上，整个功能就像Promise一样方便。

4. 接下来，我们调用函数三次，以开始获取和解码图像和文本的过程，并将每个返回的promise存储在变量中。在您之前的代码下面添加以下内容：

   ```js
   let coffee = fetchAndDecode('coffee.jpg', 'blob');
   let tea = fetchAndDecode('tea.jpg', 'blob');
   let description = fetchAndDecode('description.txt', 'text');
   ```

5. 接下来，我们将定义一个`Promise.all()`块，仅在以上存储的所有三个承诺均已成功实现时才运行某些代码。首先，在`.then()`调用中添加一个带有空执行程序的块，如下所示：

   ```js
   Promise.all([coffee, tea, description]).then(values => {
   
   });
   ```

   您会看到它接受了一个包含promise作为参数的数组。只有在所有三个诺言都解决时，执行器才会运行。当发生这种情况时，它将传递一个数组，该数组包含单个承诺（即解码后的响应主体）的结果，类似于[咖啡结果，茶结果，描述结果]。

6. 最后，在执行程序中添加以下内容。在这里，我们使用一些相当简单的同步代码将结果存储在单独的变量中（从Blob创建对象URL），然后在页面上显示图像和文本。

   ```js
   console.log(values);
   // Store each value returned from the promises in separate variables; create object URLs from the blobs
   let objectURL1 = URL.createObjectURL(values[0]);
   let objectURL2 = URL.createObjectURL(values[1]);
   let descText = values[2];
   
   // Display the images in <img> elements
   let image1 = document.createElement('img');
   let image2 = document.createElement('img');
   image1.src = objectURL1;
   image2.src = objectURL2;
   document.body.appendChild(image1);
   document.body.appendChild(image2);
   
   // Display the text in a paragraph
   let para = document.createElement('p');
   para.textContent = descText;
   document.body.appendChild(para);
   ```

7. 保存并刷新，尽管没有特别吸引人的方式，您应该看到UI组件已全部加载！

我们在此处提供的用于显示项目的代码是非常基本的，但现在可以作为解释器。

**注意**：如果遇到困难，可以将您的代码版本与我们的代码版本进行比较，以查看它的含义- [实时]( 1/learning-area/javascript/asynchronous/promises/promise-all.html)查看并查看[源代码](https://github.com/mdn/learning-area/blob/master/javascript/asynchronous/promises/promise-all.html)。

**注意**：如果要改进此代码，则可能需要循环显示要显示的项目列表，获取并解码每个结果，然后循环遍历内部结果，并`Promise.all()`根据其类型运行不同的功能以显示每个项目代码是。这将使其适用于任何数量的项目，而不仅仅是三个。

此外，您可以确定要获取的文件类型，而无需显式`type`属性。例如，您可以使用分别检查[`Content-Type`响应的HTTP标头`response.headers.get("content-type")`，然后做出相应的反应。

## 在诺言兑现/拒绝后运行一些最终代码

在某些情况下，无论承诺是否实现，您都希望在诺言完成后运行最后的代码块。以前，您必须在`.then()`和`.catch()`回调中包含相同的代码，例如：

```js
myPromise
.then(response => {
  doSomething(response);
  runFinalCode();
})
.catch(e => {
  returnError(e);
  runFinalCode();
});
```

在较新的现代浏览器中，可以使用该`.finally()`方法，该方法可以链接到常规诺言链的末尾，从而可以减少代码重复并更优雅地进行操作。上面的代码现在可以编写如下：

```js
myPromise
.then(response => {
  doSomething(response);
})
.catch(e => {
  returnError(e);
})
.finally(() => {
  runFinalCode();
});
```

这与`Promise.all()`我们在上一节中查看的演示相同，但在`fetchAndDecode()`函数中，我们将`finally()`调用链接到链的末尾：

```js
function fetchAndDecode(url, type) {
  return fetch(url).then(response => {
    if(type === 'blob') {
      return response.blob();
    } else if(type === 'text') {
      return response.text();
    }
  })
  .catch(e => {
    console.log(`There has been a problem with your fetch operation for resource "${url}": ` + e.message);
  })
  .finally(() => {
    console.log(`fetch attempt for "${url}" finished.`);
  });
}
```

这会将一条简单消息记录到控制台，以告诉我们每次获取尝试的完成时间。

**注意**：`then()`// `catch()`// `finally()`异步等效于`try`// `catch`// `finally`同步代码。

## 建立自己的自定义承诺

好消息是，在某种程度上，您已经建立了自己的承诺。当您将多个promise与`.then()`块链接在一起，或者以其他方式组合它们以创建自定义功能时，您已经在制作自己的基于异步promise的自定义功能。以`fetchAndDecode()`前面示例中的函数为例。

到目前为止，将不同的基于promise的API组合在一起以创建自定义功能是您使用promise进行自定义操作的最常见方式，并且展示了基于同一原理构建大多数现代API的灵活性和功能。但是，还有另一种方法。

### 使用Promise（）构造函数



可以使用`Promise()`构造函数来构建自己的Promise 。您要执行此操作的主要情况是，您希望基于承诺不基于承诺的老式异步API编写代码。当您需要使用现有的，较旧的项目代码，库或框架以及现代的基于诺言的代码时，这非常方便。

让我们看一个简单的示例，它可以使您入门-在这里，我们`setTimeout()`用诺言包装了调用-这会在两秒钟后运行一个函数，该函数使用`resolve()`字符串“ Success！” 来解决诺言（使用传递的调用）。

```js
let timeoutPromise = new Promise((resolve, reject) => {
  setTimeout(function(){
    resolve('Success!');
  }, 2000);
});
```

`resolve()`并且`reject()`是你打电话履行或拒绝新创建的诺言功能。在这种情况下，promise会带有字符串“ Success！”。

因此，当您调用此Promise时，您可以将一个`.then()`块链接到它的末尾，并将其传递给字符串“ Success！”。在下面的代码中，我们只是警告该消息：

```js
timeoutPromise
.then((message) => {
   alert(message);
})
```

甚至只是

```js
timeoutPromise.then(alert);
```

上面的示例不是很灵活-promise只能用单个字符串实现，并且没有`reject()`指定任何类型的条件（顺便`setTimeout()`说一句，实际上并没有失败条件，因此对此没有关系简单的例子）。

**注意**：为什么`resolve()`，而不是`fulfill()`？我们现在给您的答案*很复杂*。

### 拒绝自定义Promise



我们可以使用`reject()`方法创建一个拒绝的承诺-就像`resolve()`，它采用单个值，但是在这种情况下，这是拒绝的原因，即，错误将被传递到`.catch()`块中。

让我们扩展前面的示例，使其具有一些`reject()`条件，并允许在成功时传递不同的消息。

复制[前面的示例]()，并用以下[示例]()替换现有的`timeoutPromise()`定义：

```js
function timeoutPromise(message, interval) {
  return new Promise((resolve, reject) => {
    if (message === '' || typeof message !== 'string') {
      reject('Message is empty or not a string');
    } else if (interval < 0 || typeof interval !== 'number') {
      reject('Interval is negative or not a number');
    } else {
      setTimeout(function(){
        resolve(message);
      }, interval);
    }
  });
};
```

在这里，我们将两个参数传递给自定义函数-执行某项操作的消息以及执行该操作之前要经过的时间间隔。然后，在函数内部，我们返回一个新`Promise`对象-调用该函数将返回我们要使用的承诺。

在Promise构造函数中，我们在`if ... else`结构内部进行了一些检查：

1. 首先，我们检查消息是否适合被警报。如果它是一个空字符串，或者根本不是一个字符串，我们将以适当的错误消息拒绝promise。
2. 接下来，我们检查间隔是否为适当的间隔值。如果它是负数或不是数字，我们会以适当的错误消息拒绝诺言。
3. 最后，如果两个参数都看起来都不错，则在使用指定的时间间隔后，我们将使用指定的消息来解决Promise `setTimeout()`。

由于该`timeoutPromise()`函数返回`Promise`，我们可以链`.then()`，`.catch()`等到它要利用它的功能。现在使用它- `timeoutPromise`用此替换以前的用法：

```js
timeoutPromise('Hello there!', 1000)
.then(message => {
   alert(message);
})
.catch(e => {
  console.log('Error: ' + e);
});
```

当您按原样保存并运行代码时，一秒钟后，您将收到警报消息。现在，尝试将消息设置为空字符串或将间隔设置为负数，例如，您将能够看到Promise拒绝并显示相应的错误消息！您还可以尝试对已解决的消息进行其他操作，而不只是发出警报。

### 一个更真实的例子



上面的示例经过精心设计，以使概念易于理解，但实际上并不是非常异步。异步性质基本上是使用伪造的`setTimeout()`，尽管它仍然显示诺言对于创建具有合理操作流程，良好错误处理等的自定义函数很有用。

我们希望邀请您进行研究的一个例子`Promise()`是[Jake Archibald的idb库](https://github.com/jakearchibald/idb/)，它确实显示了构造函数的一个有用的异步应用程序。这采用了[IndexedDB API，它是一种基于回调的旧式API，用于在客户端存储和检索数据，并允许您将其与promises一起使用。如果查看[主库文件](https://github.com/jakearchibald/idb/blob/master/lib/idb.js)，将会看到上面讨论过的相同技术。以下块将许多IndexedDB方法使用的基本请求模型转换为使用Promise：

```js
function promisifyRequest(request) {
  return new Promise(function(resolve, reject) {
    request.onsuccess = function() {
      resolve(request.result);
    };

    request.onerror = function() {
      reject(request.error);
    };
  });
}
```

这可以通过添加几个事件处理程序来实现，这些事件处理程序在适当的时间实现和拒绝承诺：

- 当`request`的[`success`事件触发时，`onsuccess`处理程序将通过request履行承诺`result`。
- 当`request`的[`error`事件触发时，`onerror`处理程序会拒绝带有请求的Promise `error`。

