在本文中，我们将看到CSS可以用来设置样式控件的样式，这些样式控件更难以样式化-“坏”和“丑陋”类别。正如我们在上一篇文章中看到的那样，文本字段和按钮的样式非常简单。现在，我们将深入探讨样式问题。

| 先决条件： | 基本的计算机知识，以及对[HTML](1/en-US/docs/Learn/HTML/Introduction_to_HTML)和[CSS](1/en-US/docs/Learn/CSS/First_steps)的基本了解。 |
| :--------- | ------------------------------------------------------------ |
| 目的：     | 了解表单的哪些部分难以设置样式以及原因；学习如何定制它们。   |

回顾上一篇文章中的内容，我们有：

**坏处**：有些元素的样式更难，需要更复杂的CSS或一些更具体的技巧：

- 复选框和单选按钮
- `<input type="search">`

**丑陋的**：有些元素无法使用CSS进行彻底样式化。这些包括：

- 参与创建下拉窗口小部件，包括元素`<select>`，`<option>`，`<optgroup>`和`<datalist>`。

- `<input type="color">`

- ```
  与日期相关的控件，例如 <input type="datetime-local">
  <input type="range">
  <input type="file">
  <progress> 和 <meter>
  ```

  让我们首先谈谈该`appearance`属性，它对于使上述所有元素更具样式感非常有用。

## 外观：控制操作系统级别的样式

在上一篇文章中，我们曾说过，Web表单控件的样式大部分是从底层操作系统中获取的，这是自定义这些控件外观的问题的一部分。

[`appearance`]( /appearance)创建该属性是为了控制将哪种OS或系统级样式应用于Web窗体控件。不幸的是，此属性的原始实现的行为在不同的浏览器中是非常不同的，从而使它不太实用。较新的实现在行为上更加一致。有趣的是，基于Chromium的浏览器（Chrome，Opera，Edge），Safari和Firefox均支持`-webkit-` 前缀版本（`-webkit-appearance`）。Firefox选择了此解决方案，因为Web开发人员似乎大多使用的是`-webkit-`前缀版本，因此兼容性更好。

如果您查看参考页，则会看到列出了许多不同的可能值`-webkit-appearance`，但是到目前为止，最有用的值（也许是唯一使用的值）是`none`。这将尽可能避免您应用它的任何控件使用系统级样式，并允许您使用CSS自己构建样式。

例如，让我们进行以下控制：

```html
<form>
  <p>
    <label for="search">search: </label>
    <input id="search" name="search" type="search">
  </p>
  <p>
    <label for="text">text: </label>
    <input id="text" name="text" type="text">
  </p>
  <p>
    <label for="date">date: </label>
    <input id="date" name="date" type="datetime-local">
  </p>
  <p>
    <label for="radio">radio: </label>
    <input id="radio" name="radio" type="radio">
  </p>
  <p>
    <label for="checkbox">checkbox: </label>
    <input id="checkbox" name="checkbox" type="checkbox">
  </p>
  <p><input type="submit" value="submit"></p>
  <p><input type="button" value="button"></p>
</form>
```

将以下CSS应用于它们可删除系统级样式。

```css
input {
  -webkit-appearance: none; 
  appearance: none;
}
```

**注意**：使用前缀属性时，最好同时包含两个声明（带前缀和不带前缀）。前缀通常表示“正在进行中”，因此将来浏览器供应商可能会达成共识以删除前缀。上面的代码对于防止此类事件的将来发生是很有用的。

在大多数情况下，效果是删除了样式化的边框，这使CSS样式更加容易，但并不是必须的。在几种情况下-搜索和单选按钮/复选框，它变得更加有用。我们现在来看一下。

### 驯服搜索框

`<input type="search">`基本上只是一个文本输入，所以为什么`appearance: none;`在这里有用？答案是，在基于Chromium的浏览器中，搜索框具有一些样式限制- `height`例如，您无法调整它们。

可以使用我们的朋友来解决`appearance: none;`：

```css
input[type="search"] {
    -webkit-appearance: none;
    appearance: none;
}
```



**注意**：您可能已经注意到，在基于Chromium的浏览器和Safari中，如果输入了任何文本，则删除图标会出现在搜索输入的右侧，单击该图标会删除输入的文本。当输入在Edge和Chrome中失去焦点时，删除图标消失，但在Safari中停留。这不可能通过CSS删除-如果您的设计确实要求将其删除，则必须使用`<input type="text">`。

### 样式复选框和单选按钮



默认情况下，设置复选框或单选按钮的样式很麻烦。复选框和单选按钮的大小不希望使用其默认设计进行更改，并且当您尝试使用时，浏览器的反应会大不相同。

例如，考虑以下简单的测试案例：

```html
<input type="checkbox">
span {
    display: inline-block;
    background: red;
}

input[type="checkbox"] {
    width: 100px;
    height: 100px;
}
```

不同的浏览器以许多不同的，通常是丑陋的方式处理此问题：

| 浏览器                               | 渲染图                                                       |
| :----------------------------------- | :----------------------------------------------------------- |
| Firefox 71（macOS）                  | ![img](https://mdn.mozillademos.org/files/15671/firefox-mac-checkbox.png) |
| Firefox 57（Windows 10）             | ![img](https://mdn.mozillademos.org/files/15691/firefox-windows-checkbox.png) |
| Chrome 77（macOS），Safari 13，Opera | ![img](https://mdn.mozillademos.org/files/15676/chrome-mac-checkbox.png) |
| Chrome 63（Windows 10）              | ![img](https://mdn.mozillademos.org/files/15681/chrome-windows-checkbox.png) |
| Internet Explorer 11（Windows 10）   | ![img](https://mdn.mozillademos.org/files/15696/ie11-checkbox.png) |
| Edge 16（Windows 10）                | ![img](https://mdn.mozillademos.org/files/15686/edge-checkbox.png) |

#### 使用出现：单选框/复选框中没有

如前所述，您可以通过以下示例HTML 完全删除复选框或单选按钮的默认外观：[`appearance`]( /appearance)`:none;`

```html
<form>
  <fieldset>
    <legend>Fruit preferences</legend>

    <p>
      <label>
        <input type="checkbox" name="fruit-1" value="cherry">
        I like cherry
      </label>
    </p>
    <p>
      <label>
        <input type="checkbox" name="fruit-2" value="banana" disabled>
        I can't like banana
      </label>
    </p>
    <p>
      <label>
        <input type="checkbox" name="fruit-3" value="strawberry">
        I like strawberry
      </label>
    </p>
  </fieldset>
</form>
```

现在，让我们使用自定义复选框设计样式。让我们首先取消设置原始复选框的样式：

```css
input[type="checkbox"] {
  -webkit-appearance: none;
  appearance: none;
}
```

我们可以使用[`:checked`]( /:checked)和[`:disabled`]( /:disabled)伪类在其状态更改时更改自定义复选框的外观：

```css
input[type="checkbox"] {
  position: relative;
  width: 1em;
  height: 1em;
  border: 1px solid gray;
  /* Adjusts the position of the checkboxes on the text baseline */ 
  vertical-align: -2px;
  /* Set here so that Windows' High-Contrast Mode can override */
  color: green;
}

input[type="checkbox"]::before {
  content: "✔";
  position: absolute;
  font-size: 1.2em;
  right: -1px;
  top: -0.3em;
  visibility: hidden;
}

input[type="checkbox"]:checked::before {
  /* Use `visibility` instead of `display` to avoid recalculating layout */
  visibility: visible;
}

input[type="checkbox"]:disabled {
  border-color: black;
  background: #ddd;
  color: gray;
}
```

您将[在下一篇文章中](1/en-US/docs/Learn/Forms/UI_pseudo-classes)找到更多关于此类伪类的信息；以上这些执行以下操作：

- `:checked` —复选框（或单选按钮）处于选中状态—用户单击/激活了它。

- `:disabled` —复选框（或单选按钮）处于禁用状态—无法与之交互。

  

我们还创建了两个其他示例，为您提供更多想法：

- [样式的单选按钮]( 1/learning-area/html/forms/styling-examples/radios-styled.html)：自定义单选按钮样式。
- [拨动开关示例]( 1/learning-area/html/forms/toggle-switch-example/)：复选框的样式看起来像拨动开关。

如果您在不支持的浏览器中查看这些复选框[`appearance`]( /appearance)，则您的自定义设计将会丢失，但是它们仍然看起来像复选框并且可以使用。

**注意**：虽然Internet Explorer不支持的任何版本`appearance`，但仅`input[type=checkbox]::-ms-check`启用IE中复选框的定位。尽管名称如此，该技术也适用于单选按钮。`-ms-***check***`

## 关于“丑陋”的元素该怎么办？

现在，让我们将注意力转移到“丑陋”的控件上，这些控件确实很难彻底样式化。简而言之，它们是下拉框，复杂的控件类型（如`color`和`datetime-local`）以及面向反馈的控件（如`<progress>`和）`<meter>`。

问题在于这些元素在浏览器中的默认外观非常不同，尽管您可以通过某些方式设置其样式，但其内部的某些部分实际上无法设置样式。

如果您准备好在外观和感觉上有所不同，则可以使用一些简单的样式来使诸如背景色之类的尺寸保持一致，统一的样式，并使用外观来摆脱某些系统级样式。

下面有案例的连接

此示例对其应用了以下CSS：

```css
body {
  font-family: 'Josefin Sans', sans-serif;
  margin: 20px auto;
  max-width: 400px;
}

form > div {
  margin-bottom: 20px;
}

select {
  -webkit-appearance: none;
  appearance: none;
}

.select-wrapper {
  position: relative;
}

.select-wrapper::after {
  content: "▼";
  font-size: 1rem;
  top: 6px;
  right: 10px;
  position: absolute;
}

button, label, input, select, progress, meter {
  display: block;
  font-family: inherit;
  font-size: 100%;
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  width: 100%;
  padding: 5px;
  height: 30px;
}

input[type="text"], input[type="datetime-local"], input[type="color"], select {
  box-shadow: inset 1px 1px 3px #ccc;
  border-radius: 5px;
}

label {
  margin-bottom: 5px;
}

button {
  width: 60%;
  margin: 0 auto;
}
```

**注意**：如果要同时在多个浏览器中测试这些示例，可以[在此处实时找到它]( 1/learning-area/html/forms/styling-examples/ugly-controls.html)（也[请参见此处以获取源代码](https://github.com/mdn/learning-area/blob/master/html/forms/styling-examples/ugly-controls.html)）。

还要记住，我们已经在页面中添加了一些JavaScript，该页面在控件本身下方列出了文件选择器选择的文件。这是在`<input type="file">`参考页上找到的示例的简化版本。

如您所见，我们在使这些样式在现代浏览器中看起来统一方面做得相当不错。

我们已经对所有控件及其标签应用了一些全局规范化CSS，以使它们以相同的方式调整大小，采用其父字体等，如上一篇文章中所述：

```css
button, label, input, select, progress, meter {
  display: block;
  font-family: inherit;
  font-size: 100%;
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  width: 100%;
  padding: 5px;
  height: 30px;
}
```

我们还向有意义的控件添加了一些不规则的阴影和圆角：

```css
input[type="text"], input[type="datetime-local"], input[type="color"], select {
  box-shadow: inset 1px 1px 3px #ccc;
  border-radius: 5px;
}
```

在其他控件（例如范围类型，进度条和仪表）上，它们只是在控件区域周围添加了一个丑陋的框，因此没有任何意义。

让我们谈谈每种控制类型的一些细节，并重点说明在开发过程中遇到的困难。

### 选择和数据列表



在现代浏览器中，选择和数据列表的样式通常不会太差，只要您不希望外观和感觉与默认设置相差太大即可。

我们设法使盒子的基本外观看起来非常统一和一致。`<input type="text">`无论如何，数据列表控件都是如此，因此我们知道这将不是问题。

有两点比较麻烦。首先，跨浏览器的选择的“箭头”图标表示它是一个下拉菜单。如果您增加选择框的大小，或以丑陋的方式调整大小，它也往往会改变。为了在示例中解决此问题，我们首先使用我们的老朋友`appearance: none`完全摆脱了该图标：

```css
select {
  -webkit-appearance: none;
  appearance: none;
}
```

然后，我们使用生成的内容创建自己的图标。我们在控件周围放置了一个额外的包装器，因为`::before`/ `::after`不能在`<select>`元素上使用（这是因为生成的内容是相对于元素的格式框放置的，但是表单输入的工作方式更像是替换后的元素-它们的显示由浏览器生成并放在到位-因此没有一个）：

```html
<div class="select-wrapper"><select id="select" name="select">
  <option>Banana</option>
  <option>Cherry</option>
  <option>Lemon</option>
</select></div>
```

然后，我们使用生成的内容生成一个向下的箭头，并使用定位将其放置在正确的位置：

```css
.select-wrapper {
  position: relative;
}

.select-wrapper::after {
  content: "▼";
  font-size: 1rem;
  top: 6px;
  right: 10px;
  position: absolute;
}
```

第二个更主要的问题是，当您单击打开的`<select>`框时，您无法控制包含选项的框。您会注意到，这些选项不会继承父项上设置的字体。您也不能始终如一地设置间距和颜色等内容。例如，将应用Firefox，`color`而`background-color`在`<option>`元素上进行设置时，Chrome将不应用。它们都不会应用任何种类的间距（例如`padding`）。出现在数据列表中的自动完成列表也是如此。

如果您确实需要对选项样式的完全控制，则必须使用某种库来生成自定义控件，或者构建自己的自定义控件，或者在选择使用`multiple`属性的情况下，这将使所有选项出现在页面上，回避了此特定问题：

```html
<select id="select" name="select" multiple>
  ...
</select>
```

当然，这可能也不适合您要使用的设计，但是值得注意！

### 日期输入类型



日期/时间输入类型（`datetime-local`，`time`，`week`，`month`）都具有相同的主要相关问题。实际的包含框与任何文本输入一样容易设置样式，并且本演示中的内容看起来还不错。

但是，控件的内部部分（例如，用于选择日期的弹出式日历，可用于增加/减少值的微调器）根本无法样式化，并且无法使用摆脱它们`appearence: none;`。如果您确实需要完全控制样式，则必须使用某种库来生成自定义控件，或者构建自己的控件。

**注意**：`<input type="number">`这里也值得一提–它也有一个微调器，可用于增加/减少值，因此可能会遇到相同的问题。但是，对于这种`number`类型，所收集的数据更简单，并且`text`如果需要的话（或者`tel`如果您希望移动浏览器显示数字小键盘），仅使用输入类型也很容易。

### 范围输入类型

`<input type="range">`烦人的风格。您可以使用以下类似的方法完全删除默认的滑块轨道，并用自定义样式替换（本例中为细红色轨道）：

```css
input[type="range"] {
  appearance: none;
  -webkit-appearance: none;
  background: red;
  height: 2px;
  padding: 0;
  outline: 1px solid transparent;
}
```

但是，很难自定义范围控件的拖动手柄的样式-要完全控制范围样式，您将需要使用一堆复杂的CSS代码，包括多个非标准的，特定于浏览器的伪元素。退房[造型跨浏览器兼容范围的输入与CSS](https://css-tricks.com/styling-cross-browser-compatible-range-inputs-css/)的CSS技巧进行了详细的写入可达所需的内容。

### 颜色输入类型



颜色类型的输入控件还不错。在支持的浏览器中，它们往往只会为您提供带有小边框的纯色块。

您可以使用以下方法删除边框，只留下颜色块：

```css
input[type="color"] {
  border: 0;
  padding: 0;
}
```

但是，自定义解决方案是获得与众不同的唯一方法。

### 文件输入类型



文件类型的输入通常是可以的-正如您在我们的示例中所看到的，创建适合页面其余部分的OK相当容易-如果您告诉，作为控件一部分的输出行将继承父字体输入的信息，您可以按任何需要设置文件名和大小的自定义列表的样式；我们毕竟创建了它。

文件选择器的唯一问题是，您按打开文件选择器时所提供的按钮是完全不可样式设置的-它不能设置大小或颜色，甚至不接受其他字体。

解决此问题的一种方法是利用以下事实：如果您具有与表单控件关联的标签，则单击标签将激活该控件。因此，您可以使用类似以下内容的输入来隐藏实际内容：

```css
input[type="file"] {
  height: 0;
  padding: 0;
  opacity: 0;
}
```

然后设置标签样式，使其像按钮一样，按下按钮将按预期方式打开文件选择器：

```css
label[for="file"] {
  box-shadow: 1px 1px 3px #ccc;
  background: linear-gradient(to bottom, #eee, #ccc);
  border: 1px solid rgb(169, 169, 169);
  border-radius: 5px;
  text-align: center;
  line-height: 1.5;
}

label[for="file"]:hover {
  background: linear-gradient(to bottom, #fff, #ddd);
}

label[for="file"]:active {
  box-shadow: inset 1px 1px 3px #ccc;
}
```



### 仪表和进度条



`<meter>`而且`<progress>`可能是最糟糕的。如您在前面的示例中所看到的，我们可以相对准确地将它们设置为所需的宽度。但是除此之外，它们真的很难以任何方式进行样式设置。它们之间和浏览器之间的高度设置不一致，您可以为背景着色，但不能为前景栏着色，并且`appearance: none`对它们进行设置会使情况变得更糟，而不是更好。

如果您希望能够控制样式，或者使用第三方解决方案（例如[progressbar.js），](http://kimmobrunfeldt.github.io/progressbar.js/#examples)则只为这些功能创建自己的自定义解决方案会更容易。

## 更好的形式之路：有用的库和polyfill

如前所述，如果您想完全控制“丑陋”的控件类型，则只能依靠JavaScript。在“ [如何构建自定义表单控件”一文中，](1/en-US/docs/Learn/Forms/How_to_build_custom_form_controls)您将看到如何自己执行此操作，但是那里有一些非常有用的库可以帮助您：

- [Uni-form](http://sprawsm.com/uni-form/)是一个标准化表单标记的框架，并使用CSS对其进行样式设置。与jQuery一起使用时，它还提供了一些其他功能，但这是可选的。
- [Formalize](http://formalize.me/)是对常见JavaScript框架（例如jQuery，Dojo，YUI等）的扩展，有助于规范化和自定义表单。
- [Niceforms](http://www.emblematiq.com/lab/niceforms/)是一种独立的JavaScript方法，可提供对Web表单的完全自定义。您可以使用一些内置主题，也可以创建自己的主题。

以下库不仅与表单有关，还具有处理HTML表单的非常有趣的功能：

- [jQuery UI](http://jqueryui.com/)提供可自定义的小部件，例如日期选择器（特别注意可访问性）。
- [Twitter Bootstrap](http://twitter.github.com/bootstrap/base-css.html#forms)可以帮助您规范表格。
- [WebShim](https://afarkas.github.io/webshim/demos/)是一个巨大的工具，可以帮助您处理对浏览器HTML5的支持。Web表单部分可能真的很有帮助。

请记住，CSS和JavaScript可能会有副作用。因此，如果您选择使用这些库之一，则应始终具有健壮的后备HTML，以防脚本失败。脚本可能会失败的原因有很多，尤其是在移动世界中，并且您需要设计网站或应用程序以尽可能最好地处理这些情况。