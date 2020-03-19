到目前为止，您应该掌握了将图像、视频和音频嵌入到网页上的诀窍了。此刻，让我们继续深入学习，来看一些能让您在网页中嵌入各种内容类型的元素：`<iframe>`, `<embed>` 和`<object>`元素。`<iframe>`用于嵌入其他网页，另外两个元素则允许您嵌入PDF，SVG，甚至Flash — 一种正在被淘汰的技术，但您仍然会时不时的看到它。

| 预备知识： | 基本的计算机素养，[安装基础软件]( 1/Learn/Getting_started_with_the_web/Installing_basic_software)，[文件处理]( 1/Learn/Getting_started_with_the_web/Dealing_with_files) 的基本知识，熟悉HTML基础知识（阅读 [开始学习 HTML]( 1/docs/Learn/HTML/Introduction_to_HTML/Getting_started)）以及本模块中以前的文章。 |
| :--------- | ------------------------------------------------------------ |
| 学习目标： | 要了解如何使用`<object>`、`<embed>`以及`<iframe>`在网页中嵌入部件，例如Flash电影或其他网页。 |

## 嵌入的简史

很久以前，很流行在网络上使用**框架**创建网站 — 网站的一小部分存储于单独的HTML页面中。这些被嵌入在一个称为**框架集**的主文档中，它允许您指定每个框架能够填充在屏幕上的区域，非常像调整表格的列和行的大小。这些做法在90年代中期至90年代后期被认为是比较酷的，有证据表明，将网页分解成较小的块，这样有利于下载速度 —尤其是在那时网络连接速度太慢的情况下更为明显。然而，这些技术有很多问题，随着网络速度越来越快，这些技术带来的问题远超过它们带来的积极因素，所以你再也看不到它们被使用了。

一小段时间之后（20世纪90年代末，21世纪初），插件技术变得非常受欢迎，例如[Java Applet和[Flash — 这些技术允许网络开发者将丰富的内容嵌入到网页中，例如视频和动画等，这些内容不能通过HTML单独实现。嵌入这些技术是通过诸如`<object>`和较少使用`<embed>`的元素来实现的，当时它们非常有用。由于许多问题，包括可访问性、安全性、文件大小等，它们已经过时了; 如今，大多数移动设备不再支持这些插件，桌面端也逐渐不再支持。最后,`<iframe>`元素出现了（连同其他嵌入内容的方式，如`<canvas>`，`<video>`等），它提供了一种将整个web页嵌入到另一个网页的方法，看起来就像那个web页是另一个网页的一个`<img>`或其他元素一样。`<iframe>`现在经常被使用。





## Iframe详解

是不是很简单又有趣呢？`<iframe>`元素旨在允许您将其他Web文档嵌入到当前文档中。这很适合将第三方内容纳入您的网站，您可能无法直接控制，也不希望实现自己的版本 - 例如来自在线视频提供商的视频，[Disqus](https://disqus.com/)等评论系统，在线地图提供商，广告横幅等。

我们会在后面提到，关于`<iframe>`有一些严重的[安全隐患]( 1/docs/Learn/HTML/Multimedia_and_embedding/其他嵌入技术#安全隐患)需要考虑，但这并不意味着你不应该在你的网站上使用它们 — 它只需要一些知识和仔细地思考。让我们更详细地探索这些代码。假设您想在其中一个网页上加入MDN词汇表，您可以尝试以下方式：

```
<iframe src="1/en-US/docs/Glossary"
        width="100%" height="500" frameborder="0"
        allowfullscreen sandbox>
  <p> <a href="1/en-US/docs/Glossary">
    Fallback link for browsers that don't support iframes
  </a> </p>
</iframe>
```

此示例包括使用以下所需的`<iframe>`基本要素：

- `allowfullscreen`

  如果设置，`<iframe>`则可以通过[全屏API]( 1/docs/Web/API/Fullscreen_API)设置为全屏模式（稍微超出本文的范围）。

- `frameborder`

  如果设置为1，则会告诉浏览器在此框架和其他框架之间绘制边框，这是默认行为。0删除边框。不推荐这样设置，因为在[CSS中可以更好地实现相同的效果。[`border`]( /border)`: none;`

- `src`

  该属性与**`<video>`**/`<img>`一样包含指向要嵌入文档的URL路径。

- `width` 和 `height`

  这些属性指定您想要的iframe的宽度和高度。

- 备选内容

  与**`<video>`**等其他类似元素相同，您可以在`<iframe>`标签之间包含备选内容，如果浏览器不支持，将会显示备选内容，这种情况下，我们已经添加了一个到该页面的链接。现在您几乎不可能遇到任何不支持`<iframe>`的浏览器。

- `sandbox`

  该属性需要在已经支持其他`<iframe>`功能（例如IE 10及更高版本）但稍微更现代的浏览器上才能工作，该属性可以提高安全性设置; 我们将在下一节中更加详细地谈到。

**注意**：为了提高速度，在主内容完成加载后，使用JavaScript设置iframe的`src`属性是个好主意。这使您的页面可以更快地被使用，并减少您的官方页面加载时间（重要的[SEO指标）。

### 安全隐患



以上我们提到了安全问题 - 现在我们来详细介绍一下这一点。我们并不期望您第一次就能完全理解所有内容; 我们只想让您意识到这一问题，在您更有经验并开始考虑在您的实验和工作中使用``时为你提供参考。此外，没有必要害怕和不使用``—你只需要谨慎一点。继续看下去吧...

浏览器制造商和Web开发人员了解到网络上的坏人（通常被称为**黑客**，或更准确地说是**破解者**），如果他们试图恶意修改您的网页或欺骗人们进行不想做的事情时常把iframe作为共同的攻击目标（官方术语：**攻击向量**），例如显示用户名和密码等敏感信息。因此，规范工程师和浏览器开发人员已经开发了各种安全机制，使`<iframe>`更加安全，这有些最佳方案值得我们考虑 - 我们将在下面介绍其中的一些。

[单击劫持](https://en.wikipedia.org/wiki/Clickjacking)是一种常见的iframe攻击，黑客将隐藏的iframe嵌入到您的文档中（或将您的文档嵌入到他们自己的恶意网站），并使用它来捕获用户的交互。这是误导用户或窃取敏感数据的常见方式。

一个快速的例子 — 尝试在浏览器中加载上面的例子 - 你也可以[在Github上找到它](http://mdn.github.io/learning-area/html/multimedia-and-embedding/other-embedding-technologies/iframe-detail.html)（[参见源代码](https://github.com/mdn/learning-area/blob/gh-pages/html/multimedia-and-embedding/other-embedding-technologies/iframe-detail.html)）。你将不会看到任何内容，但如果你点击[浏览器开发者工具中的*控制台*，你会看到一条消息，告诉你为什么没有显示内容。在Firefox中，您会*被告知：“X-Frame-Options拒绝加载1/en-US/docs/Glossary”*。这是因为构建MDN的开发人员已经在网站页面的服务器上设置了一个不允许被嵌入到``的设置（请参阅[配置CSP指令]( 1/docs/Learn/HTML/Multimedia_and_embedding/Other_embedding_technologies#配置CSP指令)）这是有必要的 — 整个MDN页面被嵌入在其他页面中没有多大意义，除非您想要将其嵌入到您的网站上并将其声称为自己的内容，或尝试通过单击劫持来窃取数据，这都是非常糟糕的事情。此外，如果每个人都这样做，所有额外的带宽将花费Mozilla很多资金。

#### 只有在必要时嵌入

有时嵌入第三方内容（例如YouTube视频和地图）是有意义的，但如果您只在完全需要时嵌入第三方内容，您可以省去很多麻烦。网络安全的一个很好的经验法则是*“你怎么谨慎都不为过，如果你决定要做这件事，多检查一遍；如果是别人做的，在被证明是安全的之前，都假设这是危险的。”*

除了安全问题，你还应该意识到知识产权问题。无论在线内容还是离线内容，绝大部分内容都是有版权的，甚至是一些你没想到有版权的内容（例如，[Wikimedia Commons](https://commons.wikimedia.org/wiki/Main_Page)上的大多数图片）。不要在网页上展示一些不属于你的内容，除非你是所有者或所有者给了你明确的、书面的许可。对于侵犯版权的惩罚是严厉的。再说一次，你再小心也不为过。

如果内容获得许可，你必须遵守许可条款。例如，MDN上的内容是[在CC-BY-SA下许可的]( 1/docs/MDN/About#版权和许可)，这意味着，如果你要引用我们的内容，就必须[用适当的方式注明来源](https://wiki.creativecommons.org/wiki/Best_practices_for_attribution)，即使你对内容做了实质性的修改。

#### 使用 HTTPS

[HTTPS是[HTTP的加密版本。您应该尽可能使用HTTPS为您的网站提供服务：

1. HTTPS减少了远程内容在传输过程中被篡改的机会，
2. HTTPS防止嵌入式内容访问您的父文档中的内容，反之亦然。

使用HTTPS需要一个安全证书，这可能是昂贵的（尽管[Let's Encrypt](https://letsencrypt.org/)让这件事变得更容易），如果你没有，可以使用HTTP来为你的父文档提供服务。但是，由于HTTPS的第二个好处，*无论成本如何，您绝对不能使用HTTP嵌入第三方内容*（在最好的情况下，您的用户的Web浏览器会给他们一个可怕的警告）。所有有声望的公司，例如Google Maps或Youtube，当您嵌入内容时，``将通过HTTPS提供 - 查看`` `src`属性内的URL。

**注意**：[Github页面允许默认情况下通过HTTPS提供内容，因此对托管内容很有用。如果您正在使用不同的托管，并且不确定，请向您的托管服务商询问。

#### 始终使用`sandbox`属性

想尽可能减少攻击者在你的网站上做坏事的机会，那么你应该给嵌入的内容仅能完成自己工作的权限*。*当然，这也适用于你自己的内容。一个允许包含在其里的代码以适当的方式执行或者用于测试，但不能对其他代码库（意外或恶意）造成任何损害的容器称为[沙盒](https://en.wikipedia.org/wiki/Sandbox_(computer_security))。

未沙盒化(Unsandboxed)内容可以做得太多（执行JavaScript，提交表单，弹出窗口等）默认情况下，您应该使用没有参数的`sandbox`属性来强制执行所有可用的限制，如我们前面的示例所示。

如果绝对需要，您可以逐个添加权限（`sandbox=""`属性值内） - 请参阅`sandbox`所有可用选项的参考条目。其中重要的一点是，你*永远不*应该同时添加`allow-scripts`和`allow-same-origin`到你的`sandbox`属性中-在这种情况下，嵌入式内容可以绕过阻止站点执行脚本的同源安全策略，并使用JavaScript完全关闭沙盒。

**注意**：如果攻击者可以欺骗人们直接访问恶意内容（在iframe之外），则沙盒无法提供保护。如果某些内容可能是恶意的（例如，用户生成的内容），请保证其是从不同的[域向您的主站点提供的。

#### 配置CSP指令

[CSP代表**[内容安全策略**，它提供[一组HTTP标头（由web服务器发送时与元数据一起发送的元数据），旨在提高HTML文档的安全性。在`<iframe>`安全性方面，您可以*[将服务器配置为发送适当的`X-Frame-Options` 标题。*这样做可以防止其他网站在其网页中嵌入您的内容（这将导致[点击](https://en.wikipedia.org/wiki/clickjacking)和一系列其他攻击），正如我们之前看到的那样，MDN开发人员已经做了这些工作。



## `<embed>`和`<object>`元素

`<embed>`和`<object>`元素的功能不同于`<iframe>`—— 这些元素是用来嵌入多种类型的外部内容的通用嵌入工具，其中包括像Java小程序和Flash，PDF（可在浏览器中显示为一个PDF插件）这样的插件技术，甚至像视频，SVG和图像的内容！

**注意**：**插件**是一种对浏览器原生无法读取的内容提供访问权限的软件。

然而，您不太可能使用这些元素 - Applet几年来一直没有被使用；由于许多原因，Flash不再受欢迎（见下面的[插件案例]( 1/docs/Learn/HTML/Multimedia_and_embedding/Other_embedding_technologies#The_case_against_plugins)）；PDF更倾向于被链接而不是被嵌入；其他内容，如图像和视频都有更优秀、更容易元素来处理。插件和这些嵌入方法真的是一种传统技术，我们提及它们主要是为了以防您在某些情况下遇到问题，比如内部网或企业项目等。

如果您发现自己需要嵌入插件内容，那么您至少需要一些这样的信息：

|                                                             | `<embed>`                    | `<object>`                              |
| :---------------------------------------------------------- | :--------------------------- | :-------------------------------------- |
| 嵌入内容的[网址                 | `src`                        | `data`                                  |
| 嵌入内容的*准确*[媒体类型 | `type`                       | `type`                                  |
| 由插件控制的框的高度和宽度（以CSS像素为单位）               | `height` `width`             | `height` `width`                        |
| 名称和值，将插件作为参数提供                                | 具有这些名称和值的ad hoc属性 | 单标签`<param>`元素，包含在内`<object>` |
| 独立的HTML内容作为不可用资源的回退                          | 不支持（`<noembed>`已过时）  | 包含在元素`<obejct>`之后`<param>`       |

**注意**：`<object>`需要`data`属性，`type`属性或两者。如果您同时使用这两个，您也可以使用该`typemustmatch`属性（仅在Firefox中实现，在本文中）。`typemustmatch`保持嵌入文件不运行，除非`type`属性提供正确的媒体类型。`typemustmatch`因此，当您嵌入来自不同[来源的内容（可以防止攻击者通过插件运行任意脚本）时，可以赋予重要的安全优势。

下面是一个使用该`<embed>`元素嵌入Flash影片的示例：

```
<embed src="whoosh.swf" quality="medium"
       bgcolor="#ffffff" width="550" height="400"
       name="whoosh" align="middle" allowScriptAccess="sameDomain"
       allowFullScreen="false" type="application/x-shockwave-flash"
       pluginspage="http://www.macromedia.com/go/getflashplayer">
```

现在来看一个`<object>`将PDF嵌入一个页面的例子:

```
<object data="mypdf.pdf" type="application/pdf"
        width="800" height="1200" typemustmatch>
  <p>You don't have a PDF plugin, but you can <a href="myfile.pdf">download the PDF file.</a></p>
</object>
```

PDF是纸与数据之间重要的阶梯，但它们[在可访问性上有些问题](http://webaim.org/techniques/acrobat/acrobat)[，](http://webaim.org/techniques/acrobat/acrobat)并且可能难以在小屏幕上阅读。它们在一些圈子中仍然受欢迎，我们最好是用链接指向它们，而不是将其嵌入到网页中，以便它们可以在单独的页面上被下载或被阅读。

### 针对插件的情况



以前，插件在网络上是不可或缺的。还记得你必须安装Adobe Flash Player才能在线观看电影的日子吗？并且你还会不断地收到关于更新Flash Player和Java运行环境的烦人警报。Web技术已经变得更加强大，那些日子已经结束了。对于大多数应用程序，现在是停止依赖插件传播内容，开始利用Web技术的时候了。

- **扩大你对大家的影响力。**每个人都有一个浏览器，但插件越来越少，特别是在移动用户中。由于Web在很大程度上不需要依赖插件而运行，所以人们宁愿只是去竞争对手的网站而不是安装插件。
- **从Flash和其他插件附带的[额外的可访问性问题](http://webaim.org/techniques/flash/)中摆脱。**
- **避免额外的安全隐患。**即使经过无数次补丁[，](http://www.cvedetails.com/product/6761/Adobe-Flash-Player.html?vendor_id=53) Adobe Flash也是[非常不安全的](http://www.cvedetails.com/product/6761/Adobe-Flash-Player.html?vendor_id=53)。2015年，Facebook的首席安全官Alex Stamos甚至[要求Adobe停止Flash。](http://www.theverge.com/2015/7/13/8948459/adobe-flash-insecure-says-facebook-cso)

那你该怎么办？如果您需要交互性，HTML和[JavaScript可以轻松地为您完成工作，而不需要Java小程序或过时的ActiveX / BHO技术。您可以使用[HTML5视频]( 1/docs/Learn/HTML/Multimedia_and_embedding/Video_and_audio_content)来满足媒体需求，矢量图形[SVG]( 1/docs/Learn/HTML/Multimedia_and_embedding/Adding_vector_graphics_to_the_Web)，以及复杂图像和动画[画布]( 1/docs/Web/API/Canvas_API/Tutorial)。[彼得·埃尔斯特（Peter Elst）几年前已经提到](https://plus.google.com/+PeterElst/posts/P5t4pFhptvp)，对于工作Adobe Flash极少是正确的工具，除了专门的游戏和商业应用。对于ActiveX，即使微软的[Edge浏览器也不再支持。

## 总结

在Web文档中嵌入其他内容这一主题可以很快变得非常复杂，因此在本文中，我们尝试以一种简单而熟悉的方式来介绍它，这种介绍方式将立即显示出相关性，同时仍暗示了一些涉及更高级功能的技术。刚开始，除了嵌入第三方内容（如地图和视频），您不太可能在网页上使用到嵌入技术。当你变得更有经验时，你可能会开始为他们找到更多的用途。

除了我们在这里讨论的那些外，还有许多涉及嵌入外部内容的技术。我们看到了一些在前面的文章中出现的，如**`<video>`**，`<audio>`和`<img>`，但还有其它的有待关注，如`<canvas>`用于JavaScript生成的2D和3D图形，`<svg>`用于嵌入矢量图形。我们将在此学习模块的下一篇文章中学习[SVG。