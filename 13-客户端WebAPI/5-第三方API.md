到目前为止，我们已经介绍的API已内置在浏览器中，但并非所有API都是内置的。许多大型网站和服务（例如Google Maps，Twitter，Facebook，PayPal等）都提供了API，允许开发人员利用其数据（例如，在博客上显示Twitter流）或服务（例如，使用Facebook登录名登录用户） ）。本文着眼于浏览器API与第三方API的区别，并展示了后者的一些典型用法。

| 先决条件： | JavaScript基础知识（请参阅[第一步，[构建基块，[JavaScript对象），[客户端API的[基础知识 |
| :--------- | ------------------------------------------------------------ |
| 目的：     | 了解第三方API的工作方式，以及如何使用它们来增强您的网站。    |

## 什么是第三方API？

第三方API是第三方（通常是Facebook，Twitter或Google等公司）提供的API，可让您通过JavaScript访问其功能并在您的网站上使用。最明显的例子之一是使用映射API在页面上显示自定义地图。

让我们看一个[简单的Mapquest API示例]( 1/learning-area/javascript/apis/third-party-apis/mapquest/)，并用它来说明第三方API与浏览器API的不同之处。

**注意**：您可能只想一次[获得我们所有的代码示例](1/en-US/docs/Learn#Getting_our_code_examples)，在这种情况下，您可以只在存储库中搜索每个部分所需的示例文件。

### 在第三方服务器上找到它们



浏览器API内置在浏览器中-您可以立即从JavaScript访问它们。例如，可以使用本机对象访问[在介绍性文章中看到](1/en-US/docs/Learn/JavaScript/Client-side_web_APIs/Introduction#How_do_APIs_work)的Web Audio API [`AudioContext`。例如：

```js
const audioCtx = new AudioContext();
  ...
const audioElement = document.querySelector('audio');
  ...
const audioSource = audioCtx.createMediaElementSource(audioElement);
// etc.
```

另一方面，第三方API位于第三方服务器上。要从JavaScript访问它们，您首先需要连接到API功能并使其在您的页面上可用。这通常涉及首先通过一个`<script>`元素链接到服务器上可用的JavaScript库，如我们的Mapquest示例所示：

```js
<script src="https://api.mqcdn.com/sdk/mapquest-js/v1.3.2/mapquest.js"></script>
<link type="text/css" rel="stylesheet" href="https://api.mqcdn.com/sdk/mapquest-js/v1.3.2/mapquest.css"/>
```

然后，您可以开始使用该库中可用的对象。例如：

```js
let L;

let map = L.mapquest.map('map', {
  center: [53.480759, -2.242631],
  layers: L.mapquest.tileLayer('map'),
  zoom: 12
});
```

在这里，我们要创建一个变量来存储地图信息，然后使用`mapquest.map()`方法创建一个新地图，该方法将[``要在其中显示地图的元素的ID （“ map”）作为参数，并包含一个我们要显示的特定地图的详细信息。在这种情况下，我们指定地图中心的坐标，`map`要显示的类型的地图图层（使用`mapquest.tileLayer()`方法创建）以及默认缩放级别。

这是Mapquest API绘制简单地图所需的全部信息。您要连接的服务器可以处理所有复杂的内容，例如为显示的区域显示正确的地图图块等。



### 他们通常需要API密钥



如[我们的第一篇文章中所述](1/en-US/docs/Learn/JavaScript/Client-side_web_APIs/Introduction#They_have_additional_security_mechanisms_where_appropriate)，浏览器API的安全性通常由权限提示处理。这样做的目的是使用户知道他们访问的网站上正在发生的事情，并且不太可能成为恶意使用API的人的受害者。

第三方API的权限系统略有不同-第三方API倾向于使用开发人员密钥来允许开发人员访问API功能，这比用户更能保护API供应商。

您将在Mapquest API示例中找到与以下内容类似的行：

```html
L.mapquest.key = 'YOUR-API-KEY-HERE';
```

此行指定了要在您的应用程序中使用的API或开发人员密钥-应用程序的开发人员必须申请获取密钥，然后将其包含在其代码中，才能访问该API的功能。在我们的示例中，我们仅提供了一个占位符。

**注意**：创建自己的示例时，您将使用自己的API密钥代替任何占位符。

其他API可能会要求您以略有不同的方式包含密钥，但是大多数API的模式都相对相似。

要求密钥使API提供者可以使API用户对其行为负责。当开发者注册了密钥后，API提供者便会知道它们，并且如果他们开始对API进行任何恶意操作（例如跟踪用户的位置或尝试向API发送大量请求以向其发送垃圾邮件），则可以采取措施。例如，使其停止工作）。最简单的操作是仅撤销其API特权。

## 扩展Mapquest示例

让我们向Mapquest示例添加更多功能，以展示如何使用API的其他功能。

1. 要开始本节，请在新目录中创建[mapquest起始文件](https://github.com/mdn/learning-area/blob/master/javascript/apis/third-party-apis/mapquest/starter-file.html)的副本。如果您已经[克隆了示例存储库](1/en-US/docs/Learn#Getting_our_code_examples)，则已经有了此文件的副本，可以在*javascript / apis / third-party-apis / mapquest*目录中找到该文件的副本。
2. 接下来，您需要转到[Mapquest开发人员站点](https://developer.mapquest.com/)，创建一个帐户，然后创建一个用于您的示例的开发人员密钥。（在撰写本文时，它在网站上被称为“消费者密钥”，密钥创建过程还要求提供可选的“回调URL”。您无需在此处填写URL：只需将其留空即可）
3. 打开您的起始文件，然后用您的密钥替换API密钥占位符。

### 更改地图类型



Mapquest API可以显示许多不同类型的地图。为此，请找到以下行：

```js
layers: L.mapquest.tileLayer('map')
```

尝试更改`'map'`以`'hybrid'`显示混合样式的地图。也尝试其他一些值。该[`tileLayer`参考页](https://developer.mapquest.com/documentation/mapquest-js/v1.3/l-mapquest-tile-layer/)显示了不同的可用选项，再加上很多信息。

### 添加不同的控件



该地图具有许多可用的控件。默认情况下，它仅显示缩放控件。您可以使用`map.addControl()`方法扩展可用的控件。将此添加到您的代码中，在`window.onload`处理程序内：

```js
map.addControl(L.mapquest.control());
```

该[`mapquest.control()`方法](https://developer.mapquest.com/documentation/mapquest-js/v1.3/l-mapquest-control/)仅创建一个简单的功能齐全的控件集，默认情况下它位于右上角。您可以通过将选项对象指定为包含`position`属性的控件的参数来调整位置，该属性的值是一个字符串，用于指定控件的位置。试试这个，例如：

```js
  map.addControl(L.mapquest.control({ position: 'bottomright' }));
```

还有其他类型的控件，例如`mapquest.searchControl()`和`mapquest.satelliteControl()`，有些控件非常复杂且功能强大。玩一下，看看你能想到什么。

### 添加自定义标记



在地图上的特定位置添加标记（图标）很容易-您只需使用`L.marker()`方法（似乎已在相关Leaflet.js文档中进行了说明）。将以下代码再次添加到示例中`window.onload`：

```js
L.marker([53.480759, -2.242631], {
  icon: L.mapquest.icons.marker({
    primaryColor: '#22407F',
    secondaryColor: '#3B5998',
    shadow: true,
    size: 'md',
    symbol: 'A'
  })
})
.bindPopup('This is Manchester!')
.addTo(map);
```

如您所见，这最简单的方法有两个参数，一个是包含显示标记的坐标的数组，另一个是包含`icon`定义该点要显示图标的属性的options对象。

图标是使用`mapquest.icons.marker()`方法定义的，如您所见，该方法包含诸如标记的颜色和大小之类的信息。

在第一个方法调用的最后，我们链接`.bindPopup('This is Manchester!')`，它定义了单击标记时要显示的内容。

最后，我们链接`.addTo(map)`到链的末尾以将标记实际添加到地图。

尝试一下文档中显示的其他选项，然后看看您会想到什么！Mapquest提供了一些相当高级的功能，例如方向，搜索等。



## Google Maps呢？

Google Maps可以说是最受欢迎的Maps API，那么为什么在我们的地图示例中不使用它呢？我们确实[创建了一个示例](https://github.com/mdn/learning-area/blob/master/javascript/apis/third-party-apis/google-maps/finished-maps-example.html)来展示如何使用它，但是最后我们出于几个原因选择了Mapquest：

- 入门非常容易。通常，对于Google API，您需要创建一个Google帐户并登录到[Google Cloud Platform Console](https://console.cloud.google.com/)来创建API密钥等，并且此过程相当复杂。特别是对于[Google Maps API](https://cloud.google.com/maps-platform/)，您需要提供用于计费的信用卡（尽管基本使用是免费的），但是我们认为基本教程不接受这种信用卡。
- 我们想证明还有其他选择。

## RESTful API — NYTimes

现在，让我们来看另一个API示例- [纽约时报API](https://developer.nytimes.com/)。使用此API，您可以检索《纽约时报》的新闻报道信息，并将其显示在您的网站上。这种类型的API被称为**RESTful API-**与使用Mapquest一样，我们不使用JavaScript库的功能来获取数据，而是通过对特定URL发出HTTP请求来获取数据，而搜索字词和其他属性则以URL（通常作为URL参数）。这是API会遇到的常见模式。

## 一种使用第三方API的方法

下面我们将通过一个练习向您展示如何使用NYTimes API，该操作还提供了一套更通用的步骤，您可以将其用作使用新API的一种方法。

### 查找文档



当您要使用第三方API时，必须找出文档的位置，以便您可以找到API的功能，使用方式等。《纽约时报》 API文档位于[https：// /developer.nytimes.com/](https://developer.nytimes.com/)。

### 获取开发人员密钥



出于安全性和责任制的原因，大多数API都要求您使用某种开发人员密钥。要注册NYTimes API密钥，请按照[https://developer.nytimes.com/get-started上](https://developer.nytimes.com/get-started)的说明进行操作。

1. 让我们为Article Search API请求一个密钥-创建一个新应用，选择它作为您要使用的API（填写名称和描述，将“ Article Search API”下的开关切换到on位置，然后单击“创建”）。
2. 从结果页面获取API密钥。
3. 现在，要开始示例，请在计算机上的新目录中复制[nytimes_start.html](https://github.com/mdn/learning-area/blob/master/javascript/apis/third-party-apis/nytimes/nytimes_start.html)和[nytimes.css](https://github.com/mdn/learning-area/blob/master/javascript/apis/third-party-apis/nytimes/nytimes.css)。如果您已经[克隆了示例存储库](1/en-US/docs/Learn#Getting_our_code_examples)，那么您将已经拥有这些文件的副本，可以在*javascript / apis / third-party-apis / nytimes*目录中找到它们。

该应用程序最终将允许您输入搜索词以及可选的开始和结束日期，然后将其用于查询Article Search API并显示搜索结果。

![nytimes-example](https://mdn.mozillademos.org/files/17025/nytimes-example.png)

### 将API连接到您的应用



首先，您需要在API和您的应用之间建立连接。对于此API，每次从正确的URL向服务请求数据时，都需要将API密钥作为[get参数包括在内。

1. 找到以下行：

   ```js
   let key = ' ... ';
   ```

   用您在上一节中获得的实际API密钥替换现有的API密钥。

2. 将以下行添加到JavaScript的“ `// Event listeners to control the functionality`”注释下方。`submitSearch()`提交表单（按下按钮）时，它将运行一个称为的函数。

   ```js
   searchForm.addEventListener('submit', submitSearch);
   ```

3. 现在，在前一行下面添加`submitSearch()`和`fetchResults()`函数定义：

   ```
   函数commitSearch（e）{
     pageNumber = 0;
     fetchResults（e）;
   }
   
   函数fetchResults（e）{
     //使用preventDefault（）停止提交表单
     e.preventDefault（）;
   
     //组装完整的URL
     url = baseURL +'？api-key ='+键+'＆page ='+ pageNumber +'＆q ='+ searchTerm.value + ' ＆fq = document_type :(“ article”）'  ;
   
     if（startDate.value！==''）{
       url + ='＆begin_date ='+ startDate.value;
     };
   
     if（endDate.value！==''）{
       url + ='＆end_date ='+ endDate.value;
     };
   
   }
   ```

`submitSearch()`将页码从0开始重新设置，然后调用`fetchResults()`。这首先调用`preventDefault()`事件对象，以停止实际提交的表单（这将破坏示例）。接下来，我们使用一些字符串操作来组合将发出请求的完整URL。我们首先组装该演示中我们认为必不可少的零件：

- 基本网址（来自`baseURL`变量）。
- API密钥，必须在`api-key`URL参数中指定（该值取自`key`变量）。
- 页码，必须在`page`URL参数中指定（该值取自`pageNumber`变量）。
- 搜索项，必须在`q`URL参数中指定（该值取自`searchTerm`text 的值`<input>`）。
- 要返回结果的文档类型，如通过`fq`URL参数传入的表达式所指定。在这种情况下，我们要退货。

接下来，我们使用几个`if()`语句来检查`startDate`和是否`endDate` ``在其上填充了值。如果是这样，我们会将其值附加到分别在`begin_date`和`end_date`参数中指定的URL。

因此，完整的URL最终看起来像这样：

```
https://api.nytimes.com/svc/search/v2/articlesearch.json?api-key=YOUR-API-KEY-HERE&page=0&q=cats
＆fq = document_type :(“文章”）＆begin_date = 20170301＆end_date = 20170312
```

**注意**：您可以在[NYTimes开发人员文档中](https://developer.nytimes.com/)找到有关可以包含哪些URL参数的更多详细信息。

**注意**：该示例具有基本的表单数据验证功能-必须先填写搜索词字段，然后才能提交表单（使用`required`属性来实现），并且date字段具有`pattern`指定的属性，这意味着除非它们的值，否则它们不会提交由8个数字（`pattern="[0-9]{8}"`）组成。

### 从API请求数据



现在，我们已经构造了URL，让我们对其进行请求。我们将使用[Fetch API进行此操作。

在`fetchResults()`函数内的右花括号上方添加以下代码块：

```js
// Use fetch() to make the request to the API
fetch(url).then(function(result) {
  return result.json();
}).then(function(json) {
  displayResults(json);
});
```

在这里，我们通过将`url`变量传递给来运行请求，`fetch()`使用`json()`函数将响应主体转换为JSON ，然后将结果JSON传递给`displayResults()`函数，以便可以在我们的UI中显示数据。

### 显示数据



好的，让我们看看如何显示数据。在函数下方添加以下`fetchResults()`函数。

```
函数displayResults（json）{
  while（section.firstChild）{
      section.removeChild（section.firstChild）;
  }

  const article = json.response.docs;

  if（articles.length === 10）{
    nav.style.display ='block';
  }其他{
    nav.style.display ='none';
  }

  if（articles.length === 0）{
    const para = document.createElement（'p'）;
    para.textContent ='未返回结果。'
    section.appendChild（para）;
  }其他{
    for（var i = 0; i <article.length; i ++）{
      const article = document.createElement（'article'）;
      const heading = document.createElement（'h2'）;
      const link = document.createElement（'a'）;
      const img = document.createElement（'img'）;
      const para1 = document.createElement（'p'）;
      const para2 = document.createElement（'p'）;
      const clearfix = document.createElement（'div'）;

      让current =文章[i];
      console.log（当前）;

      link.href = current.web_url;
      link.textContent = current.headline.main;
      para1.textContent =当前。片段 ;
      para2.textContent ='关键字：';
      for（let j = 0; j <current.keywords.length; j ++）{
        const span = document.createElement（'span'）;
        span.textContent + = current.keywords [j] .value +'';
        para2.appendChild（span）;
      }

      if（current.multimedia.length> 0）{
        img.src ='http://www.nytimes.com/'+ current.multimedia [0] .url;
        img.alt = current.headline.main;
      }

      clearfix.setAttribute（'class'，'clearfix'）;

      article.appendChild（heading）;
      heading.appendChild（link）;
      article.appendChild（img）;
      article.appendChild（para1）;
      article.appendChild（para2）;
      article.appendChild（clearfix）;
      section.appendChild（article）;
    }
  }
}
```

这里有很多代码。让我们逐步解释一下：

- 该`while`环是用来删除所有的DOM元素的内容的共同的模式，在这种情况下，`<section>`元件。我们会继续检查，看看是否`<section>`有第一个孩子，如果有，我们将删除第一个孩子。当`<section>`不再有任何子代时，循环结束。

- 接下来，我们将`articles`变量设置为相等`json.response.docs`-这是一个数组，其中包含代表搜索返回的文章的所有对象。这样做纯粹是为了使以下代码更简单。

- 第`if()`一块检查是否返回了10篇文章（API一次最多返回10篇文章。）如果是，我们将显示`<nav>`包含“ *前10个* /后*10个”*分页按钮的。如果返回的文章少于10条，则它们都将合计在一页上，因此我们无需显示分页按钮。在下一节中，我们将分页功能。

- 下一个`if()`块检查是否没有文章返回。如果是这样，我们将不尝试显示任何内容，而是创建一个`<p>`包含文本“未返回结果”的。并将其插入`<section>`

- 如果返回了一些文章，我们首先创建我们要用来显示每个新闻故事的所有元素，将正确的内容插入每个新闻中，然后将它们插入到DOM中的适当位置。为了弄清楚article对象中的哪些属性包含要显示的正确数据，我们查阅了Article Search API参考（请参阅

  NYTimes API

  ）。这些操作大多数都很明显，但有些值得一提：

  - 我们使用了[for循环（`for(var j = 0; j < current.keywords.length; j++) { ... }`）来遍历与每篇文章相关的所有关键字，并将每一个关键字插入其自己的`<span>`，之中`<p>`。这样做是为了简化每个样式的样式。
  - 我们使用`if()`块（`if(current.multimedia.length > 0) { ... }`）检查每篇文章是否有任何与之相关的图像（有些故事没有）。我们仅在第一幅图像存在的情况下显示它（否则会引发错误）。
  - 我们为`<div>`元素提供了一个“ clearfix”类，因此我们可以轻松地对其应用清除。

### 连接分页按钮



为了使分页按钮正常工作，我们将增加（或减少）`pageNumber`变量的值，然后使用页面URL参数中包含的新值重新运行获取请求。之所以可行，是因为NYTimes API一次仅返回10个结果-如果有10个以上的结果，则将`page`URL参数设置为0（或完全不包含）时，它将返回前10个（0-9）。默认值），如果设置为1，则下一个10（10-19），依此类推。

这使我们可以轻松编写简单的分页函数。

1. 在现有`addEventListener()`调用下面，添加这两个新调用，这将导致在单击相关按钮时调用`nextPage()`和`previousPage()`函数：

   ```js
   nextBtn.addEventListener('click', nextPage);
   previousBtn.addEventListener('click', previousPage);
   ```

2. 在您之前添加的内容下面，让我们定义两个函数-现在添加此代码：

   ```js
   function nextPage(e) {
     pageNumber++;
     fetchResults(e);
   };
   
   function previousPage(e) {
     if(pageNumber > 0) {
       pageNumber--;
     } else {
       return;
     }
     fetchResults(e);
   };
   ```

   第一个函数很简单-我们递增`pageNumber`变量，然后`fetchResults()`再次运行该函数以显示下一页的结果。

   第二个函数的反向操作几乎完全相同，但是我们还必须采取额外的步骤检查`pageNumber`递减之前尚未为零的值-如果获取请求使用负`page`URL参数运行，则可能会导致错误。如果a `pageNumber`已经为0，我们就简单地`return`退出该函数，以避免浪费处理能力（如果已经在第一页，则无需再次加载相同的结果）。

**注意**：您可以[在GitHub上](https://github.com/mdn/learning-area/blob/master/javascript/apis/third-party-apis/nytimes/index.html)找到我们[完成的nytimes API示例代码](https://github.com/mdn/learning-area/blob/master/javascript/apis/third-party-apis/nytimes/index.html)（也[可以在此处实时运行]( 1/learning-area/javascript/apis/third-party-apis/nytimes/)）。

## YouTube示例

我们还构建了另一个示例供您学习和学习-请参阅我们的[YouTube视频搜索示例]( 1/learning-area/javascript/apis/third-party-apis/youtube/)。这使用两个相关的API：

- 在[YouTube数据API](https://developers.google.com/youtube/v3/docs/)来搜索YouTube视频，并返回结果。
- 在[YouTube上的IFrame播放器API](https://developers.google.com/youtube/iframe_api_reference)来显示IFrame的视频播放器内返回视频的例子，所以你可以看他们。

这个示例很有趣，因为它显示了两个相关的第三方API一起用于构建应用程序。第一个是RESTful API，第二个则更像Mapquest（使用特定于API的方法等）。但是，值得注意的是，这两个API均需要将JavaScript库应用于该页面。RESTful API具有可用于处理发出HTTP请求并返回结果的功能。

![img](https://mdn.mozillademos.org/files/14823/youtube-example.png)

在本文中，我们将不对这个示例进行过多说明- [源代码](https://github.com/mdn/learning-area/tree/master/javascript/apis/third-party-apis/youtube)在其中插入了详细的注释，以解释其工作原理。

要使其运行，您需要：

- 从[Google Cloud](https://cloud.google.com/)获取API密钥。
- `ENTER-API-KEY-HERE`在源代码中找到字符串，并将其替换为您的API密钥。
- 通过Web服务器运行示例。如果仅在浏览器中直接运行它（即通过`file://`URL），它将无法工作。

