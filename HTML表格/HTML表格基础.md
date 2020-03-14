本文将从HTML表格开始，介绍一些基本的内容，如行和单元格、标题、使单元格跨越多个列和行，以及如何将列中的所有单元组合在一起进行样式化。

| 前置知识: | HTML基本概念 (参见 [Introduction to HTML](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML))。 |
| :-------- | ------------------------------------------------------------ |
| 目标:     | 了解熟悉HTML表格基本知识。                                   |

## 什么是表格？

表格是由行和列组成的结构化数据集(表格数据)，它能够使你简捷迅速地查找某个表示不同类型数据之间的某种关系的值 。比如说，某个人和他的年龄，一天或是一周，当地游泳池的时间表 。

![A sample table showing names and ages of some people - Chris 38, Dennis 45, Sarah 29, Karen 47.](https://mdn.mozillademos.org/files/14583/numbers-table.png)

![A swimming timetable showing a sample data table](https://mdn.mozillademos.org/files/14587/swimming-timetable.png)

表格在人类社会中很常见，而且已经存在很长时间了，下面这张1800年的美国人口普查文件中就可以证明：

![A very old parchment document; the data is not easily readable, but it clearly shows a data table being used.](https://mdn.mozillademos.org/files/14585/1800-census.jpg)

因此，HTML的创建者们提供了一种方法来构建和呈现web上的表格数据，这也就不足为奇了。

### 表格风格



你可以在 GitHub 上找到上面表格的 [HTML源码](https://github.com/mdn/learning-area/blob/master/html/tables/basic/personal-pronouns.html) ; 先去看看, 当然也可以看看这个 [look at the live example](http://mdn.github.io/learning-area/html/tables/basic/personal-pronouns.html)! 你也许会注意到一件事情，那就是这个表格看上去可读性不是很好，那是因为现在这个页面上面的那个表格通过 MDN 站点添加了一些样式, 而 GitHub 上面的并没有添加。

不要幻想; 为了能够让表格在网页上有效, 你需要提供一些 CSS 的样式信息，以及尽可能好的 HTML 固定结构. 在这个模块中，我们将专注于 HTML 部分; 在你完成这里的内容之后，你可以浏览 [Styling tables](https://developer.mozilla.org/en-US/docs/Learn/CSS/Styling_boxes/Styling_tables) 来了解 CSS 的部分。

虽然在这个模块中我们不会专注于 CSS, 但是我们提供了一个较小的 CSS 样式表让你使用，和默认的没有采用任何 CSS 样式的表相比，表格会更加可读。 你可以在 [stylesheet here](https://github.com/mdn/learning-area/blob/master/html/tables/basic/minimal-table.css) 获取样式表，以及在 [HTML template](https://github.com/mdn/learning-area/blob/master/html/tables/basic/blank-template.html) 获取 HTML 文件来应用样式表，这些会让你在 “测试 HTML 表格” 中有一个好的起点。

**注意**: 也可以看下 [personal_pronouns table with this styling applied](http://mdn.github.io/learning-area/html/tables/basic/personal-pronouns-styled.html) 这个版本, 这个是应用了 CSS 以后表格看上去的样子。

### 什么时候你不应该使用 HTML 表格?



HTML 表格 应该用于表格数据 ，这正是 HTML 表格设计出来的用途. 不幸的是, 许多人习惯用 HTML 表格来实现网页布局， e.g. 一行包含 header, 一行包含几列内容, 一行包含 footer, etc. 你可以在我们的 [Accessibility Learning Module](https://developer.mozilla.org/en-US/docs/Learn/Accessibility) 中的 [Page Layouts](https://developer.mozilla.org/en-US/docs/Learn/Accessibility/HTML#Page_layouts) 获得更多细节内容和一个示例。这种做法以前是很常见的，因为以前 CSS 在不同浏览器上的兼容性比较糟糕 ; 表格布局现在不太普遍，但您可能仍然会在网络的某些角落看到它们。

简单来说, 使用表格布局而不使用 [CSS layout techniques](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout) 是很糟糕的. 主要的理由有以下几个:

1. **表格布局减少了视觉受损的用户的可访问性**: [屏幕阅读器](https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Cross_browser_testing/Accessibility#Screenreaders), 被盲人所使用, 解析存在于 HTML 页面上的标签，然后为用户读出其中的内容。因为对于布局来说，表格不是一个正确的工具， 使用的标记比使用 CSS 布局技术更复杂, 所以屏幕阅读器的输出会让他们的用户感到困惑。
2. **表格会产生很多标签**: 正如刚才提到的, 表格布局通常会比正确的布局技术涉及更复杂的标签结构，这会导致代码变得更难于编写、维护、调试.
3. **表格不能自动响应**: 当你使用正确的布局容器 (比如 <header>, <section>, <article>, 或是<div>), 它们的默认宽度是父元素的 100%. 而表格的的默认大小是根据其内容而定的。因此，需要采取额外的措施来获取表格布局样式，以便有效地在各种设备上工作。

## 使用 <th> 元素添加标题

现在，让我们把注意力转向表格标题，表格中的标题是特殊的单元格，通常在行或列的开始处，定义行或列包含的数据类型 (举个例子, 看到本篇文章中第一个示例中的 "单数" 或者 "Object"  ). 为了说明它们为什么这么有用, 来看下面这个例子，首先是源代码:

```html
<table>
  <tr>
    <td>&nbsp;</td>
    <td>Knocky</td>
    <td>Flor</td>
    <td>Ella</td>
    <td>Juan</td>
  </tr>
  <tr>
    <td>Breed</td>
    <td>Jack Russell</td>
    <td>Poodle</td>
    <td>Streetdog</td>
    <td>Cocker Spaniel</td>
  </tr>
  <tr>
    <td>Age</td>
    <td>16</td>
    <td>9</td>
    <td>10</td>
    <td>5</td>
  </tr>
  <tr>
    <td>Owner</td>
    <td>Mother-in-law</td>
    <td>Me</td>
    <td>Me</td>
    <td>Sister-in-law</td>
  </tr>
  <tr>
    <td>Eating Habits</td>
    <td>Eats everyone's leftovers</td>
    <td>Nibbles at food</td>
    <td>Hearty eater</td>
    <td>Will eat till he explodes</td>
  </tr>
</table>
```

这是表格实际呈现的效果:

|               | Knocky                    | Flor            | Ella         | Juan                      |
| ------------- | ------------------------- | --------------- | ------------ | ------------------------- |
| Breed         | Jack Russell              | Poodle          | Streetdog    | Cocker Spaniel            |
| Age           | 16                        | 9               | 10           | 5                         |
| Owner         | Mother-in-law             | Me              | Me           | Sister-in-law             |
| Eating Habits | Eats everyone's leftovers | Nibbles at food | Hearty eater | Will eat till he explodes |

这里的问题是：虽然你可以弄清楚发生了什么，但是尽可能的交叉参考数据并不容易。如果列和行的标题以某种方式出现，那将会更好。

### 为什么标题是有用的?

我们已经给出了部分答案，当标题明显突出的时候，你可以更加简单地找到你想找的数据，设计上也会看起来更好。

**注意**: 即使你不给表格添加你自己的样式，表格标题也会带有一些默认样式：加粗和居中，让标题可以突出显示。

表格标题也有额外的好处，随着 `scope` 属性 (我们将在下一篇文章中了解到)，这个属性允许你让表格变得更加无障碍，每个标题与相同行或列中的所有数据相关联。屏幕阅读设备能一次读出一列或一行的数据，这是非常有帮助的。

## 允许单元格跨越多行和列

有时我们希望单元格跨越多行或多列。以下是一个简单的例子，显示了一些常见动物的名字。在某些情况下，我们要显示动物名称旁边的男性和女性的名字。有时候我们又不需要，那不需要的情况下，我们希望写着动物的名字的单元格的宽度可以是两个单元格的宽度 (因为写着名字的行会有两列，而没有写名字的行只有一列，行的宽度是不一样的)。

一开始的标记写法是这样的:

```html
<table>
  <tr>
    <th>Animals</th>
  </tr>
  <tr>
    <th>Hippopotamus</th>
  </tr>
  <tr>
    <th>Horse</th>
    <td>Mare</td>
  </tr>
  <tr>
    <td>Stallion</td>
  </tr>
  <tr>
    <th>Crocodile</th>
  </tr>
  <tr>
    <th>Chicken</th>
    <td>Cock</td>
  </tr>
  <tr>
    <td>Rooster</td>
  </tr>
</table>
```

但是输出的结果不是我们想要的:

| Animals      |      |
| :----------- | ---- |
| Hippopotamus |      |
| Horse        | Mare |
| Stallion     |      |
| Crocodile    |      |
| Chicken      | Hen  |
| Rooster      |      |

我们需要一个方法，让 "Animals", "Hippopotamus", 和 "Crocodile" 的单元格的宽度变为两个单元格， "Horse" 和 "Chicken" 的高度变为两行 (因为要拥有一个男性名字和女性名字，可以先看效果图)。幸好, 表格中的标题和单元格有 `colspan` 和 `rowspan` 属性，这两个属性可以帮助我们实现这些效果。这两个属性接受一个没有单位的数字值，数字决定了它们的宽度或高度是几个单元格。比如, `colspan="2"` 使一个单元格的宽度是两个单元格。

让我们使用 `colspan` 和 `rowspan` 来改进现有的表格。

1. 首先，把 [animals-table.html](https://github.com/mdn/learning-area/blob/master/html/tables/basic/animals-table.html) 和 [minimal-table.css](https://github.com/mdn/learning-area/blob/master/html/tables/basic/minimal-table.css) 文件复制到你的本地环境，HTML 文件中包含了你刚才看到的动物示例的数据。
2. 接着，使用 `colspan` 让 "Animals", "Hippopotamus", 和 "Crocodile" 占 2 个单元格的宽度。
3. 最后，使用 `rowspan` 让 "Horse" 和 "Chicken" 占 2 个单元格的高度。
4. 保存后，用浏览器打开你写的 HTML 文件，看看改进的地方。

**注意**: 你也可以在 GitHub 上找到完成的版本 [animals-table-fixed.html](https://github.com/mdn/learning-area/blob/master/html/tables/basic/animals-table-fixed.html) ([see it live also](https://mdn.github.io/learning-area/html/tables/basic/animals-table-fixed.html)).

## 为表格中的列提供共同的样式

在我们继续介绍之前，我们将介绍本文中的最后一个功能。HTML有一种方法可以定义整列数据的样式信息：就是<col>和<colgroup>元素。 它们存在是因为如果你想让一列中的每个数据的样式都一样，那么你就要为每个数据都添加一个样式，这样的做法是令人厌烦和低效的。你通常需要在列中的每个<td>或 <th>上定义样式，或者使用一个复杂的选择器，比如 `:nth-child()`。

下面是一个简单的示例:

```html
<table>
  <tr>
    <th>Data 1</th>
    <th style="background-color: yellow">Data 2</th>
  </tr>
  <tr>
    <td>Calcutta</td>
    <td style="background-color: yellow">Orange</td>
  </tr>
  <tr>
    <td>Robots</td>
    <td style="background-color: yellow">Jazz</td>
  </tr>
</table>
```

下面就是上述代码的结果:

| Data 1   | Data 2 |
| :------- | :----- |
| Calcutta | Orange |
| Robots   | Jazz   |

这样不太理想，因为我们不得不在列中的每个单元格中重复那些样式信息 (在真实的项目中，我们或许会设置一个 `class` 包含那三个单元格 ，然后在一个单独的样式表中定义样式). 为了舍弃这种做法，我们可以只定义一次，在 <col>元素中。 <colgroup>元素被规定包含在 容器中，而 <colgroup>就在<table>标签的下方。我们可以通过如下的做法来创建与上面相同的效果:

```html
<table>
  <colgroup>
    <col>
    <col style="background-color: yellow">
  </colgroup>
  <tr>
    <th>Data 1</th>
    <th>Data 2</th>
  </tr>
  <tr>
    <td>Calcutta</td>
    <td>Orange</td>
  </tr>
  <tr>
    <td>Robots</td>
    <td>Jazz</td>
  </tr>
</table>
```

我们使用了两个<col>来定义“列的样式”，每一个<col>都会制定每列的样式，对于第一列，我们没有采取任何样式，但是我们仍然需要添加一个空的<col>元素，如果不这样做，那么我们的样式就会应用到第一列上，这和我们预想的不一样。

如果你想把这种样式信息应用到每一列，我们可以只使用一个<col>元素，不过需要包含 span 属性，像这样：

```html
<colgroup>
  <col style="background-color: yellow" span="2">
</colgroup>
```

就像 `colspan` 和 `rowspan`, `span` 需要一个无单位的数字值，用来制定你想要让这个样式应用到表格中多少列