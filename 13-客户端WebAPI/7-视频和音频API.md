HTML5带有用于在文档中嵌入富媒体的元素- `<video>`和`<audio>`-进而具有它们自己的用于控制播放，搜索等的API。本文向您展示如何执行常见任务，例如创建自定义播放控件。

| 先决条件： | JavaScript基础知识（请参阅第一步，构建基块，JavaScript对象），客户端API的基础知识 |
| :--------- | ------------------------------------------------------------ |
| 目的：     | 了解如何使用浏览器API来控制视频和音频播放。                  |

## HTML5视频和音频

在`<video>`和` <audio>`元素使我们能够嵌入视频和音频到网页。如我们在 视频和音频内容中所示，典型的实现如下所示：

```html
<video controls>
  <source src="rabbit320.mp4" type="video/mp4">
  <source src="rabbit320.webm" type="video/webm">
  <p>Your browser doesn't support HTML5 video. Here is a <a href="rabbit320.mp4">link to the video</a> instead.</p>
</video>
```



您可以在上面链接的文章中回顾所有HTML功能。对于我们这里的目的，最有趣的属性是`controls`，它启用了默认的播放控件集。如果不指定此选项，则不会获得播放控件.



这对于视频播放不是立即有用的，但是确实具有优势。本机浏览器控件的一个大问题是每个浏览器中的控件都不相同-对于跨浏览器的支持不是很好！另一个大问题是，大多数浏览器中的本机控件不是很容易通过键盘访问的。

您可以通过隐藏本机控件（通过删除`controls`属性）并使用HTML，CSS和JavaScript编程自己的控件来解决这两个问题。在下一节中，我们将介绍可用于执行此操作的基本工具。

## HTMLMediaElement API

HTML5规范的一部分，该`HTMLMediaElement`API提供的功能，让您可以编程控制的视频和音频播放器-例如`HTMLMediaElement.play()`，`HTMLMediaElement.pause()`等此接口可用于两个`<audio>`和`<video>`元素，功能，你会希望实现几乎相同。

### 入门



#### 探索HTML

打开HTML索引文件。您会看到许多功能。HTML由视频播放器及其控件控制：

```html
<div class="player">
  <video controls>
    <source src="video/sintel-short.mp4" type="video/mp4">
    <source src="video/sintel-short.webm" type="video/webm">
    <!-- fallback content here -->
  </video>
  <div class="controls">
    <button class="play" data-icon="P" aria-label="play pause toggle"></button>
    <button class="stop" data-icon="S" aria-label="stop"></button>
    <div class="timer">
      <div></div>
      <span aria-label="timer">00:00</span>
    </div>
    <button class="rwd" data-icon="B" aria-label="rewind"></button>
    <button class="fwd" data-icon="F" aria-label="fast forward"></button>
  </div>
</div>
```

- 整个播放器都包裹在一个`<div>`元素中，因此如果需要，可以将其全部设置为一个单元。
- 该`<video>`元素包含两个`<source>`元素，以便可以根据查看站点的浏览器来加载不同的格式。
- HTML控件可能是最有趣的：
  - 我们有4秒-播放/暂停，停止，倒带和快进。
  - 每个`<button>`都有一个`class`名称，一个`data-icon`用于定义应在每个按钮上显示哪些图标的属性（我们将在下面的部分中显示其工作原理），以及一个`aria-label`提供每个按钮的可理解描述的属性，因为我们没有提供标签内的人类可读标签。`aria-label`当屏幕阅读器的用户关注包含它们的元素时，属性的内容就会被屏幕阅读器读出。
  - 还有一个计时器`<div>`，它将报告视频播放时所经过的时间。只是为了好玩，我们提供了两种报告机制–一种`<span>`包含以分钟和秒为单位的经过时间，另一种`<div>`是我们将用来创建水平指示条的附加机制，该指示条随着时间的流逝而变长。

#### 探索CSS

现在打开CSS文件，并查看内部。该示例的CSS不太复杂，但是我们将在此处重点介绍最有趣的部分。首先，请注意`.controls`样式：

```css
.controls {
  visibility: hidden;
  opacity: 0.5;
  width: 400px;
  border-radius: 10px;
  position: absolute;
  bottom: 20px;
  left: 50%;
  margin-left: -200px;
  background-color: black;
  box-shadow: 3px 3px 5px black;
  transition: 1s all;
  display: flex;
}

.player:hover .controls, player:focus .controls {
  opacity: 1;
}
```

- 我们首先`visibility`将自定义控件的设置为`hidden`。稍后在JavaScript中，我们将控件设置为`visible`，然后`controls`从`<video>`元素中删除属性。这样一来，如果由于某种原因无法加载JavaScript，则用户仍可以将视频与本机控件一起使用。
- `opacity`默认情况下，我们将控件设置为0.5，这样在您尝试观看视频时，它们的注意力就会减少。只有当您将鼠标悬停在播放器上或将焦点对准播放器时，控件才会完全不透明。
- 我们使用Flexbox（`display`：flex）在控制栏内布置按钮，以使事情变得更容易。

接下来，让我们看一下按钮图标：

```css
@font-face {
   font-family: 'HeydingsControlsRegular';
   src: url('fonts/heydings_controls-webfont.eot');
   src: url('fonts/heydings_controls-webfont.eot?#iefix') format('embedded-opentype'),
        url('fonts/heydings_controls-webfont.woff') format('woff'),
        url('fonts/heydings_controls-webfont.ttf') format('truetype');
   font-weight: normal;
   font-style: normal;
}

button:before {
  font-family: HeydingsControlsRegular;
  font-size: 20px;
  position: relative;
  content: attr(data-icon);
  color: #aaa;
  text-shadow: 1px 1px 0px black;
}
```

首先，在CSS的顶部，我们使用一个`@font-face`块来导入自定义Web字体。这是一种图标字体-字母表中的所有字符都等同于您可能希望在应用程序中使用的常用图标。

接下来，我们使用生成的内容在每个按钮上显示一个图标：

- 我们使用`::before`选择器在每个`<button>`元素之前显示内容。
- 我们使用该`content`属性将每种情况下要显示的内容设置为等于该`data-icon`属性的内容。在我们的播放按钮的情况下，`data-icon`包含大写字母“ P”。
- 我们使用将自定义网络字体应用于按钮`font-family`。在此字体中，“ P”实际上是“播放”图标，因此播放按钮上显示有“播放”图标。

图标字体之所以非常酷，有很多原因-减少了HTTP请求，因为您不需要将这些图标下载为图像文件，可伸缩性强，并且可以使用文本属性来设置它们的样式。

最后但并非最不重要的一点，让我们看一下计时器的CSS：

```css
.timer {
  line-height: 38px;
  font-size: 10px;
  font-family: monospace;
  text-shadow: 1px 1px 0px black;
  color: white;
  flex: 5;
  position: relative;
}

.timer div {
  position: absolute;
  background-color: rgba(255,255,255,0.2);
  left: 0;
  top: 0;
  width: 0;
  height: 38px;
  z-index: 2;
}

.timer span {
  position: absolute;
  z-index: 3;
  left: 19px;
}
```

- 我们将外部设置`.timer` `<div>`为具有flex：5，因此它占据了控制栏的大部分宽度。我们还给出了，以便我们可以根据元素的边界而不是元素的边界方便地将元素放置在其中。`position`: `relative`,`<body>`
- 内部`<div>`绝对定位为直接位于外部之上。它的初始宽度也为0，因此您根本看不到它。随着视频的播放，随着时间的流逝，宽度会通过JavaScript增大。
- 该`<span>`也绝对定位坐在附近的计时器栏的左侧。
- 我们还提供内部`<div>`和`<span>`适当数量`z-index`的计时器，以便计时器显示在顶部，内部显示在`<div>`底部。这样，我们确保可以看到所有信息-一个框不会遮挡另一个框。

### 实施JavaScript



我们已经有了一个相当完整的HTML和CSS界面；现在我们只需要连接所有按钮即可使控件正常工作。

1. 在与index.html文件相同的目录级别中创建一个新的JavaScript文件。叫它`custom-player.js`。

2. 在此文件的顶部，插入以下代码：

   ```js
   const media = document.querySelector('video');
   const controls = document.querySelector('.controls');
   
   const play = document.querySelector('.play');
   const stop = document.querySelector('.stop');
   const rwd = document.querySelector('.rwd');
   const fwd = document.querySelector('.fwd');
   
   const timerWrapper = document.querySelector('.timer');
   const timer = document.querySelector('.timer span');
   const timerBar = document.querySelector('.timer div');
   ```

   在这里，我们正在创建常量，以保存对我们要操作的所有对象的引用。我们分为三组：

   - ```
     <video>元件，并且控制杆。
     ```

   - 播放/暂停，停止，快退和快进按钮。

   - 外部计时器包装器`<div>`,数字计时器读数`<span>`和内部计时器`<div>`随着时间的流逝而变宽。

3. 接下来，在代码底部插入以下内容：

   ```js
   media.removeAttribute('controls');
   controls.style.visibility = 'visible';
   ```

   这两行从视频中删除了默认的浏览器控件，并使自定义控件可见。

#### 播放和暂停视频

让我们实现最重要的控件-播放/暂停按钮。

1. 首先，将以下内容添加到代码的底部，以便在`playPauseMedia()`单击播放按钮时调用该函数：

   ```js
   play.addEventListener('click', playPauseMedia);
   ```

2. 现在定义`playPauseMedia()`-在代码底部再次添加以下内容：

   ```js
   function playPauseMedia() {
     if(media.paused) {
       play.setAttribute('data-icon','u');
       media.play();
     } else {
       play.setAttribute('data-icon','P');
       media.pause();
     }
   }
   ```

   在这里，我们使用一条`if`语句来检查视频是否已暂停。`HTMLMediaElement.paused`如果媒体暂停（在任何时候都没有播放视频，包括在第一次加载后将其设置为0时），则该属性返回true。如果已暂停，`data-icon`则将播放按钮上的属性值设置为“ u”（这是一个“已暂停”图标），并调用该`HTMLMediaElement.play()`方法来播放媒体。

   第二次单击时，该按钮将再次切换回-“播放”图标将再次显示，并且视频将以暂停`HTMLMediaElement.paused()`。

#### 停止视频

1. 接下来，让我们添加一些功能来处理视频的停止。在`addEventListener()`您添加的上一行下面添加以下几行：

   ```js
   stop.addEventListener('click', stopMedia);
   media.addEventListener('ended', stopMedia);
   ```

   该`click`事件是显而易见的-我们希望通过我们的运行停止视频`stopMedia()`被点击停止按钮时的功能。但是，我们也确实想在视频播放完后停止播放-这由`ended`事件触发来标记，因此我们还设置了一个侦听器，以在事件触发时运行该函数。

2. 接下来，让我们定义`stopMedia()`—在下面添加以下功能`playPauseMedia()`：

   ```js
   function stopMedia() {
     media.pause();
     media.currentTime = 0;
     play.setAttribute('data-icon','P');
   }
   ```

   `stop()`HTMLMediaElement API上没有方法-等效于`pause()`视频，并将其`currentTime`属性设置`currentTime`为0。设置为值（以秒为单位）会立即将媒体跳转到该位置。

   之后要做的就是将显示的图标设置为“播放”图标。无论按下暂停按钮是暂停视频还是播放视频，您都希望以后可以开始播放。

#### 来回寻求

您可以通过多种方式实现快退和快进功能。在这里，我们向您展示了一种相对复杂的方法，当以意外的顺序按下不同的按钮时，该方法不会中断。

1. 首先，`addEventListener()`在前面的两行下面添加以下两行：

   ```js
   rwd.addEventListener('click', mediaBackward);
   fwd.addEventListener('click', mediaForward);
   ```

2. 现在进入事件处理程序函数-在您之前的函数下面添加以下代码以定义`mediaBackward()`和`mediaForward()`：

   ```js
   let intervalFwd;
   let intervalRwd;
   
   function mediaBackward() {
     clearInterval(intervalFwd);
     fwd.classList.remove('active');
   
     if(rwd.classList.contains('active')) {
       rwd.classList.remove('active');
       clearInterval(intervalRwd);
       media.play();
     } else {
       rwd.classList.add('active');
       media.pause();
       intervalRwd = setInterval(windBackward, 200);
     }
   }
   
   function mediaForward() {
     clearInterval(intervalRwd);
     rwd.classList.remove('active');
   
     if(fwd.classList.contains('active')) {
       fwd.classList.remove('active');
       clearInterval(intervalFwd);
       media.play();
     } else {
       fwd.classList.add('active');
       media.pause();
       intervalFwd = setInterval(windForward, 200);
     }
   }
   ```

   您会注意到，首先我们初始化两个变量- `intervalFwd`和`intervalRwd`-您将在以后找到它们的含义。

   让我们逐步介绍一下`mediaBackward()`（功能`mediaForward()`完全相同，但是相反）：

   1. 我们清除快进功能上设置的所有类别和时间间隔–之所以这样做，是因为如果在按下`rwd`按钮后按下`fwd`按钮，我们想取消任何快进功能并将其替换为快退功能。如果我们试图同时做到这两者，那么玩家会破产。
   2. 我们使用一条`if`语句来检查是否在按钮`active`上设置了该类`rwd`，表明该类已经被按下。这`classList`是一个非常方便的属性，它存在于每个元素上-它包含该元素上设置的所有类的列表以及添加/删除类的`classList.contains()`方法等。我们使用该方法检查列表中是否包含`active`该类。这将返回一个布尔值`true`/ `false`结果。
   3. 如果`active`已在`rwd`按钮上设置，我们将使用删除它`classList.remove()`，清除第一次按下按钮时设置的间隔（有关更多说明，请参见下文），然后使用`HTMLMediaElement.play()`取消快退并正常开始播放视频。
   4. 如果尚未设置，我们使用将该`active`类添加到`rwd`按钮上，使用`classList.add()`暂停视频`HTMLMediaElement.pause()`，然后将该`intervalRwd`变量设置为等于`setInterval()`通话。调用时，将`setInterval()`创建一个活动间隔，这意味着它将每x毫秒运行作为第一个参数给出的函数，其中x是第二个参数的值。因此，这里我们`windBackward()`每200毫秒运行一次该函数-我们将使用此函数不断向后倒退视频。要停止`setInterval()`运行，您必须调用`clearInterval()`，为其指定要清除的时间间隔的标识名称，在这种情况下，该名称是变量名称`intervalRwd`（请参见`clearInterval()`函数中前面的调用）。

3. 最后，我们需要定义在调用中调用的`windBackward()`和`windForward()`函数`setInterval()`。将以下内容添加到之前的两个功能下面：

   ```js
   function windBackward() {
     if(media.currentTime <= 3) {
       rwd.classList.remove('active');
       clearInterval(intervalRwd);
       stopMedia();
     } else {
       media.currentTime -= 3;
     }
   }
   
   function windForward() {
     if(media.currentTime >= media.duration - 3) {
       fwd.classList.remove('active');
       clearInterval(intervalFwd);
       stopMedia();
     } else {
       media.currentTime += 3;
     }
   }
   ```

   再次，我们将遍历这些功能中的第一个，因为它们的工作原理几乎相同，但彼此相反。在`windBackward()`执行以下操作时-请记住，当间隔处于活动状态时，此函数每200毫秒运行一次。

   1. 我们以一条`if`语句开始，该语句检查当前时间是否少于3秒，即，如果再倒转3秒，它将回到视频的开头。这会导致奇怪的行为，因此，在这种情况下，我们可以通过调用停止视频播放`stopMedia()`，`active`从倒带按钮中删除该类，并清除`intervalRwd`间隔以停止倒带功能。如果我们不做最后一步，那么视频将永远倒带。
   2. 如果当前时间不在视频开始播放的3秒之内，我们只需执行即可从当前时间删除3秒`media.currentTime -= 3`。因此，实际上，我们将视频每3毫秒后退3秒。

#### 更新经过的时间

我们的媒体播放器要实现的最后一部分是经过的时间显示。为此，我们将运行一个函数，以在每次`timeupdate`在`<video>`元素上触发事件时更新时间显示。

`addEventListener()`在其他行下方添加以下行：

```js
media.addEventListener('timeupdate', setTime);
```

现在定义`setTime()`函数。在文件底部添加以下内容：

```js
function setTime() {
  let minutes = Math.floor(media.currentTime / 60);
  let seconds = Math.floor(media.currentTime - minutes * 60);
  let minuteValue;
  let secondValue;

  if (minutes < 10) {
    minuteValue = '0' + minutes;
  } else {
    minuteValue = minutes;
  }

  if (seconds < 10) {
    secondValue = '0' + seconds;
  } else {
    secondValue = seconds;
  }

  let mediaTime = minuteValue + ':' + secondValue;
  timer.textContent = mediaTime;

  let barLength = timerWrapper.clientWidth * (media.currentTime/media.duration);
  timerBar.style.width = barLength + 'px';
}
```

这是一个相当长的函数，所以让我们逐步进行一下操作：

1. 首先，我们计算出该值的分钟和秒数`HTMLMediaElement.currentTime`。
2. 然后，我们再初始化两个变量- `minuteValue`和`secondValue`。
3. 这两个`if`语句可以计算出分钟数和秒数是否小于10。如果是，则它们会以数字时钟显示的相同方式在值上添加前导零。
4. 要显示的实际时间值设置为`minuteValue`加冒号加`secondValue`。
5. `Node.textContent`计时器的值设置为时间值，因此它显示在UI中。
6. ``首先，通过计算外部的宽度``（任何元素的`clientWidth`属性将包含其长度），然后乘以`HTMLMediaElement.currentTime`除以`HTMLMediaElement.duration`介质的总和，得出应该设置内部的长度。
7. 我们将内部的宽度设置等于所计算的条形长度加上“ px”，因此它将被设置为该像素数。

#### 固定播放和暂停

还有一个问题需要解决。如果在快退或快进功能处于活动状态时按下播放/暂停或停止按钮，则它们将不起作用。我们如何解决该问题，以便他们取消`rwd`/ `fwd`按钮功能并按您的期望播放/停止视频？这很容易解决。

首先，在`stopMedia()`函数内添加以下几行—任何地方都可以：

```js
rwd.classList.remove('active');
fwd.classList.remove('active');
clearInterval(intervalRwd);
clearInterval(intervalFwd);
```

现在，在`playPauseMedia()`函数的开始处（在`if`语句开始之前）再次添加相同的行。

此时，您可以从`windBackward()`和`windForward()`函数中删除等效行，因为该功能已在函数中实现`stopMedia()`。

注意：您还可以通过创建运行这些行的单独函数，然后在需要的任何地方调用该函数，而不是在代码中多次重复这些行，来进一步提高代码的效率。但是，我们将由您自己决定。

