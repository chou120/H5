以下兼容性表尝试总结CSS对HTML表单的支持状态。由于CSS和HTML表单的复杂性，这些表不能被视为完美的参考。但是，它们将使您深入了解可以做什么和不能做什么，这将帮助您学习如何做事。

## 如何阅读表单

### 价值观



对于每个属性，有四个可能的值：

- 是

  跨浏览器对该属性有相当一致的支持。在某些情况下，您可能仍会面临奇怪的副作用。

- 部分的

  该属性起作用时，您可能经常会遇到奇怪的副作用或不一致之处。除非先掌握这些副作用，否则您应该避免使用这些属性。

- 没有

  该属性根本不起作用或不一致，以至于不可靠。

- 呐

  该属性对于这种类型的小部件没有意义。

### 渲染图



对于每个属性，都有两种可能的渲染：

- N（正常）

  指示按原样应用属性

- T（已调整）

  指示该属性已应用以下额外规则：

```css
* {
  /* Turn off the native look and feel */
  -webkit-appearance: none;
  appearance: none;

/* for Internet Explorer */
  background: none;
}
```

## 相容性表

### 全球行为



在全球范围内，某些行为对于许多浏览器来说是常见的：

- `border`，`background`，`border-radius`，`height`

  使用这些属性之一可以部分或完全关闭某些浏览器上小部件的本机外观。使用它们时要小心。

- `line-height`

  跨浏览器不一致地支持此属性，因此应避免使用它。

- `text-decoration`

  Opera在表单窗口小部件上不支持此属性。

- `text-overflow`

  Opera，Safari和IE9在表单小部件上不支持此属性。

- `text-shadow`

  Opera不支持`text-shadow`表单小部件，而IE9则完全不支持。

### 文字栏位



见`text`，`search`和`password`输入类型。

| 属性              |   ñ    |   Ť    | 注意                                                         |
| :---------------- | :----: | :----: | :----------------------------------------------------------- |
| *CSS盒子模型*     |        |        |                                                              |
| `width`           |   是   |   是   |                                                              |
| `height`          |  部分  |   是   | WebKit浏览器（大多数在Mac OSX和iOS上）使用本机外观作为搜索字段。因此，需要使用`-webkit-appearance:none`才能将此属性应用于搜索字段。在Windows 7上，除非`background:none`已应用，否则Internet Explorer 9不会应用边框。 |
| `border`          |  部分  |   是   | WebKit浏览器（大多数在Mac OSX和iOS上）使用本机外观作为搜索字段。因此，需要使用`-webkit-appearance:none`才能将此属性应用于搜索字段。在Windows 7上，除非`background:none`已应用，否则Internet Explorer 9不会应用边框。 |
| `margin`          |   是   |   是   |                                                              |
| `padding`         |  部分  |   是   | WebKit浏览器（大多数在Mac OSX和iOS上）使用本机外观作为搜索字段。因此，需要使用`-webkit-appearance:none`才能将此属性应用于搜索字段。在Windows 7上，除非`background:none`已应用，否则Internet Explorer 9不会应用边框。 |
| *文字和字体*      |        |        |                                                              |
| `color`           |   是   |   是   | 如果`border-color`未设置属性，则某些基于WebKit的浏览器会将`color`属性应用于边框。 |
| `font`            |   是   |   是   |                                                              |
| `letter-spacing`  |   是   |   是   |                                                              |
| `text-align`      |   是   |   是   |                                                              |
| `text-decoration` | 部分的 | 部分的 | 请参阅有关Opera的说明                                        |
| `text-indent`     |  部分  |  部分  | IE9支持此属性，而Opera仅在单行文本字段上支持此属性。         |
| `text-overflow`   | 部分的 | 部分的 |                                                              |
| `text-shadow`     | 部分的 | 部分的 |                                                              |
| `text-transform`  |   是   |   是   |                                                              |
| *边框和背景*      |        |        |                                                              |
| `background`      |  部分  |   是   | WebKit浏览器（大多数在Mac OSX和iOS上）使用本机外观作为搜索字段。因此，需要使用`-webkit-appearance:none`才能将此属性应用于搜索字段。在Windows 7上，除非`background:none`已应用，否则Internet Explorer 9不会应用边框。 |
| `border-radius`   |  部分  |   是   | WebKit浏览器（大多数在Mac OSX和iOS上）使用本机外观作为搜索字段。因此，需要使用`-webkit-appearance:none`才能将此属性应用于搜索字段。在Windows 7上，除非`background:none`已应用，否则Internet Explorer 9不会应用边框。在Opera上，`border-radius`仅当设置了显式边框时才应用该属性。 |
| `box-shadow`      |  没有  |  部分  | IE9不支持此属性。                                            |

### `button`



见`button`， `submit`以及`reset`输入类型和`<button>`元件。

| 属性              |   ñ    |   Ť    | 注意                                                         |
| :---------------- | :----: | :----: | :----------------------------------------------------------- |
| *CSS盒子模型*     |        |        |                                                              |
| `width`           |   是   |   是   |                                                              |
| `height`          |  部分  |   是   | 此属性不适用于Mac OSX或iOS上基于WebKit的浏览器。             |
| `border`          | 部分的 |   是   |                                                              |
| `margin`          |   是   |   是   |                                                              |
| `padding`         |  部分  |   是   | 此属性不适用于Mac OSX或iOS上基于WebKit的浏览器。             |
| *文字和字体*      |        |        |                                                              |
| `color`           |   是   |   是   |                                                              |
| `font`            |   是   |   是   |                                                              |
| `letter-spacing`  |   是   |   是   |                                                              |
| `text-align`      |  没有  |  没有  |                                                              |
| `text-decoration` | 部分的 |   是   |                                                              |
| `text-indent`     |   是   |   是   |                                                              |
| `text-overflow`   |  没有  |  没有  |                                                              |
| `text-shadow`     | 部分的 | 部分的 |                                                              |
| `text-transform`  |   是   |   是   |                                                              |
| *边框和背景*      |        |        |                                                              |
| `background`      |   是   |   是   |                                                              |
| `border-radius`   |  是的  |  是的  | 在Opera上，`border-radius`仅当设置了显式边框时才应用该属性。 |
| `box-shadow`      |  没有  |  部分  | IE9不支持此属性。                                            |

### 数



请参阅 `number`输入类型。没有标准的方法可以更改用于更改字段值的微调框样式，而Safari上的微调框位于字段之外。

| 属性              |   ñ    |   Ť    | 注意                                               |
| :---------------- | :----: | :----: | :------------------------------------------------- |
| *CSS盒子模型*     |        |        |                                                    |
| `width`           |   是   |   是   |                                                    |
| `height`          |  部分  |  部分  | 在Opera上，微调框被放大，可以隐藏字段的内容。      |
| `border`          |   是   |   是   |                                                    |
| `margin`          |   是   |   是   |                                                    |
| `padding`         |  部分  |  部分  | 在Opera上，微调框被放大，可以隐藏字段的内容。      |
| *文字和字体*      |        |        |                                                    |
| `color`           |   是   |   是   |                                                    |
| `font`            |   是   |   是   | 请参阅有关的注释`line-height`。                    |
| `letter-spacing`  |   是   |   是   |                                                    |
| `text-align`      |   是   |   是   |                                                    |
| `text-decoration` | 部分的 | 部分的 |                                                    |
| `text-indent`     |   是   |   是   |                                                    |
| `text-overflow`   |  没有  |  没有  |                                                    |
| `text-shadow`     | 部分的 | 部分的 |                                                    |
| `text-transform`  | 不适用 | 不适用 |                                                    |
| *边框和背景*      |        |        |                                                    |
| `background`      |  没有  |  没有  | 支持，但浏览器之间存在太多不一致之处，因此不可靠。 |
| `border-radius`   |  没有  |  没有  |                                                    |
| `box-shadow`      |  没有  |  没有  |                                                    |

### 复选框和单选按钮



请参阅`checkbox`和`radio`输入类型。

| 属性              |   ñ    |   Ť    | 注意                                                 |
| :---------------- | :----: | :----: | :--------------------------------------------------- |
| *CSS盒子模型*     |        |        |                                                      |
| `width`           |   否   |   否   | 一些浏览器增加了额外的边距，而另一些则扩展了小部件。 |
| `height`          |   否   |   否   | 一些浏览器增加了额外的边距，而另一些则扩展了小部件。 |
| `border`          |  没有  |  没有  |                                                      |
| `margin`          |   是   |   是   |                                                      |
| `padding`         |  没有  |  没有  |                                                      |
| *文字和字体*      |        |        |                                                      |
| `color`           | 不适用 | 不适用 |                                                      |
| `font`            | 不适用 | 不适用 |                                                      |
| `letter-spacing`  | 不适用 | 不适用 |                                                      |
| `text-align`      | 不适用 | 不适用 |                                                      |
| `text-decoration` | 不适用 | 不适用 |                                                      |
| `text-indent`     | 不适用 | 不适用 |                                                      |
| `text-overflow`   | 不适用 | 不适用 |                                                      |
| `text-shadow`     | 不适用 | 不适用 |                                                      |
| `text-transform`  | 不适用 | 不适用 |                                                      |
| *边框和背景*      |        |        |                                                      |
| `background`      |  没有  |  没有  |                                                      |
| `border-radius`   |  没有  |  没有  |                                                      |
| `box-shadow`      |  没有  |  没有  |                                                      |

### 选择框（单行）

见`<select>`，`<optgroup>`和 `<option>`元素。

| 属性              |    ñ    |    Ť    | 注意                                                         |
| :---------------- | :-----: | :-----: | :----------------------------------------------------------- |
| *CSS盒子模型*     |         |         |                                                              |
| `width`           |  部分1  |  部分1  | ``元素上的此属性尚可，但``or ``元素上的情况并非如此。        |
| `height`          |  没有   |   是    |                                                              |
| `border`          | 部分的  |   是    |                                                              |
| `margin`          |   是    |   是    |                                                              |
| `padding`         |   否1   |  部分2  | 该属性已应用，但在Mac OSX上的浏览器之间以不一致的方式使用。该属性很好地应用于了``元素，但是在``和``元素上却不一致地对其进行了处理。 |
| *文字和字体*      |         |         |                                                              |
| `color`           |  部分1  |  部分1  | 在Mac OSX上，基于WebKit的浏览器不支持本机窗口小部件的此属性，它们与Opera一起完全不支持on ``和``element。 |
| `font`            |  部分1  |  部分1  | 在Mac OSX上，基于WebKit的浏览器不支持本机窗口小部件的此属性，它们与Opera一起完全不支持on ``和``element。 |
| `letter-spacing`  |  部分1  |  部分1  | IE9不支持此属性``，``和``要素; Mac OSX上基于WebKit的浏览器不支持``和``元素上的此属性。 |
| `text-align`      |   否1   |   否1   | Windows 7上的IE9和Mac OSX上的基于WebKit的浏览器不支持此小部件上的此属性。 |
| `text-decoration` |  部分1  |  部分1  | 仅Firefox为此属性提供完全支持。Opera根本不支持此属性，其他浏览器仅在``元素上支持此属性。 |
| `text-indent`     | 部分1 2 | 部分1 2 | 大多数浏览器仅在``元素上支持此属性。IE9不支持此属性。        |
| `text-overflow`   |  没有   |  没有   |                                                              |
| `text-shadow`     | 部分1 2 | 部分1 2 | 大多数浏览器仅在``元素上支持此属性。IE9不支持此属性。        |
| `text-transform`  |  部分1  |  部分1  | 大多数浏览器仅在``元素上支持此属性。                         |
| *边框和背景*      |         |         |                                                              |
| `background`      |  部分1  |  部分1  | 大多数浏览器仅在``元素上支持此属性。                         |
| `border-radius`   |  部分1  |  部分1  |                                                              |
| `box-shadow`      |  没有   |  部分1  |                                                              |

注意Firefox不提供任何更改`<select>`元素上向下箭头的方法。

### 选择框（多行）



请参见`<select>`,`<optgroup>`和 `<option>`元素以及`size`属性。

| 属性              |   ñ   |   Ť   | 注意                                                         |
| :---------------- | :---: | :---: | :----------------------------------------------------------- |
| *CSS盒子模型*     |       |       |                                                              |
| `width`           |  是   |  是   |                                                              |
| `height`          |  是   |  是   |                                                              |
| `border`          |  是   |  是   |                                                              |
| `margin`          |  是   |  是   |                                                              |
| `padding`         | 部分  | 部分  | Opera不支持`padding-top`，并`padding-bottom`在上``元素。     |
| *文字和字体*      |       |       |                                                              |
| `color`           |  是   |  是   |                                                              |
| `font`            |  是   |  是   | 请参阅有关的注释`line-height`。                              |
| `letter-spacing`  | 部分  | 部分  | IE9不支持此属性``，``和``要素; Mac OSX上基于WebKit的浏览器不支持``和``元素上的此属性。 |
| `text-align`      |  否   |  否   | Windows 7上的IE9和Mac OSX上的基于WebKit的浏览器在此小部件上不支持此属性。 |
| `text-decoration` |  否   |  否   | 仅受Firefox和IE9 +支持。                                     |
| `text-indent`     | 没有  | 没有  |                                                              |
| `text-overflow`   | 没有  | 没有  |                                                              |
| `text-shadow`     | 没有  | 没有  |                                                              |
| `text-transform`  | 部分1 | 部分1 | 大多数浏览器仅在``元素上支持此属性。                         |
| *边框和背景*      |       |       |                                                              |
| `background`      |  是   |  是   |                                                              |
| `border-radius`   | 是的  | 是的  | 在Opera上，`border-radius`仅当设置了显式边框时才应用该属性。 |
| `box-shadow`      | 没有  | 部分  | IE9不支持此属性。                                            |

### 资料清单



请参见`<datalist>`和`<input>`元素以及`list`属性。

| 属性              |  ñ   |  Ť   | 注意 |
| :---------------- | :--: | :--: | :--- |
| *CSS盒子模型*     |      |      |      |
| `width`           | 没有 | 没有 |      |
| `height`          | 没有 | 没有 |      |
| `border`          | 没有 | 没有 |      |
| `margin`          | 没有 | 没有 |      |
| `padding`         | 没有 | 没有 |      |
| *文字和字体*      |      |      |      |
| `color`           | 没有 | 没有 |      |
| `font`            | 没有 | 没有 |      |
| `letter-spacing`  | 没有 | 没有 |      |
| `text-align`      | 没有 | 没有 |      |
| `text-decoration` | 没有 | 没有 |      |
| `text-indent`     | 没有 | 没有 |      |
| `text-overflow`   | 没有 | 没有 |      |
| `text-shadow`     | 没有 | 没有 |      |
| `text-transform`  | 没有 | 没有 |      |
| *边框和背景*      |      |      |      |
| `background`      | 没有 | 没有 |      |
| `border-radius`   | 没有 | 没有 |      |
| `box-shadow`      | 没有 | 没有 |      |

### 文件选择器



请参阅`file`输入类型。

| 属性              |  ñ   |  Ť   | 注意                                             |
| :---------------- | :--: | :--: | :----------------------------------------------- |
| *CSS盒子模型*     |      |      |                                                  |
| `width`           | 没有 | 没有 |                                                  |
| `height`          | 没有 | 没有 |                                                  |
| `border`          | 没有 | 没有 |                                                  |
| `margin`          |  是  |  是  |                                                  |
| `padding`         | 没有 | 没有 |                                                  |
| *文字和字体*      |      |      |                                                  |
| `color`           |  是  |  是  |                                                  |
| `font`            |  否  |  否  | 受支持，但浏览器之间存在太多不一致，因此不可靠。 |
| `letter-spacing`  | 部分 | 部分 | 许多浏览器将此属性应用于选择按钮。               |
| `text-align`      | 没有 | 没有 |                                                  |
| `text-decoration` | 没有 | 没有 |                                                  |
| `text-indent`     | 部分 | 部分 | 它的作用或多或少就像是小部件外部的额外左边界。   |
| `text-overflow`   | 没有 | 没有 |                                                  |
| `text-shadow`     | 没有 | 没有 |                                                  |
| `text-transform`  | 没有 | 没有 |                                                  |
| *边框和背景*      |      |      |                                                  |
| `background`      |  否  |  否  | 受支持，但浏览器之间存在太多不一致，因此不可靠。 |
| `border-radius`   | 没有 | 没有 |                                                  |
| `box-shadow`      | 没有 | 部分 | IE9不支持此属性。                                |

### 日期选择器



请参阅`date`和`time`输入类型。支持许多属性，但为了确保可靠性，浏览器之间存在很多不一致性。

| 属性              |  ñ   |  Ť   | 注意 |
| :---------------- | :--: | :--: | :--- |
| *CSS盒子模型*     |      |      |      |
| `width`           | 没有 | 没有 |      |
| `height`          | 没有 | 没有 |      |
| `border`          | 没有 | 没有 |      |
| `margin`          |  是  |  是  |      |
| `padding`         | 没有 | 没有 |      |
| *文字和字体*      |      |      |      |
| `color`           | 没有 | 没有 |      |
| `font`            | 没有 | 没有 |      |
| `letter-spacing`  | 没有 | 没有 |      |
| `text-align`      | 没有 | 没有 |      |
| `text-decoration` | 没有 | 没有 |      |
| `text-indent`     | 没有 | 没有 |      |
| `text-overflow`   | 没有 | 没有 |      |
| `text-shadow`     | 没有 | 没有 |      |
| `text-transform`  | 没有 | 没有 |      |
| *边框和背景*      |      |      |      |
| `background`      | 没有 | 没有 |      |
| `border-radius`   | 没有 | 没有 |      |
| `box-shadow`      | 没有 | 没有 |      |

### 拾色器



请参阅`color`输入类型：

| 属性              |   ñ    |   Ť    | 注意                                                |
| :---------------- | :----: | :----: | :-------------------------------------------------- |
| *CSS盒子模型*     |        |        |                                                     |
| `width`           |   是   |   是   |                                                     |
| `height`          |   否   |   是   | Opera像选择小部件一样处理此问题，但具有相同的限制。 |
| `border`          |   是   |   是   |                                                     |
| `margin`          |   是   |   是   |                                                     |
| `padding`         |   否   |   是   | Opera像选择小部件一样处理此问题，但具有相同的限制。 |
| *文字和字体*      |        |        |                                                     |
| `color`           | 不适用 | 不适用 |                                                     |
| `font`            | 不适用 | 不适用 |                                                     |
| `letter-spacing`  | 不适用 | 不适用 |                                                     |
| `text-align`      | 不适用 | 不适用 |                                                     |
| `text-decoration` | 不适用 | 不适用 |                                                     |
| `text-indent`     | 不适用 | 不适用 |                                                     |
| `text-overflow`   | 不适用 | 不适用 |                                                     |
| `text-shadow`     | 不适用 | 不适用 |                                                     |
| `text-transform`  | 不适用 | 不适用 |                                                     |
| *边框和背景*      |        |        |                                                     |
| `background`      |   否   |   否   | 受支持，但浏览器之间存在太多不一致，因此不可靠。    |
| `border-radius`   |   否   |   否   |                                                     |
| `box-shadow`      |   否   |   否   |                                                     |

### 仪表与进度



请参阅`<meter>`和`<gropress>`元素：

| 属性              |   ñ    |   Ť    | 注意                                                         |
| :---------------- | :----: | :----: | :----------------------------------------------------------- |
| *CSS盒子模型*     |        |        |                                                              |
| `width`           |   是   |   是   |                                                              |
| `height`          |   是   |   是   |                                                              |
| `border`          | 部分的 |   是   |                                                              |
| `margin`          |   是   |   是   |                                                              |
| `padding`         |   是   |  部分  | 将属性应用于经过调整的元素时，Chrome会隐藏``and 元素。padding` |
| *文字和字体*      |        |        |                                                              |
| `color`           | 不适用 | 不适用 |                                                              |
| `font`            | 不适用 | 不适用 |                                                              |
| `letter-spacing`  | 不适用 | 不适用 |                                                              |
| `text-align`      | 不适用 | 不适用 |                                                              |
| `text-decoration` | 不适用 | 不适用 |                                                              |
| `text-indent`     | 不适用 | 不适用 |                                                              |
| `text-overflow`   | 不适用 | 不适用 |                                                              |
| `text-shadow`     | 不适用 | 不适用 |                                                              |
| `text-transform`  | 不适用 | 不适用 |                                                              |
| *边框和背景*      |        |        |                                                              |
| `background`      |   否   |   否   | 受支持，但浏览器之间存在太多不一致，因此不可靠。             |
| `border-radius`   |   否   |   否   |                                                              |
| `box-shadow`      |   否   |   否   |                                                              |

### 范围



请参阅`range`输入类型。没有标准的方法可以更改范围夹点的样式，Opera也无法调整范围小部件的默认渲染。

| 属性              |   ñ    |   Ť    | 注意                                                         |
| :---------------- | :----: | :----: | :----------------------------------------------------------- |
| *CSS盒子模型*     |        |        |                                                              |
| `width`           |   是   |   是   |                                                              |
| `height`          |  部分  | 部分1  | Chrome和Opera会在小部件周围增加一些额外的空间，而Windows 7上的Opera会扩大范围。 |
| `border`          |  没有  |   是   |                                                              |
| `margin`          |   是   |   是   |                                                              |
| `padding`         |  部分  |   是   | 该`padding`被应用，但没有视觉效果。                          |
| *文字和字体*      |        |        |                                                              |
| `color`           | 不适用 | 不适用 |                                                              |
| `font`            | 不适用 | 不适用 |                                                              |
| `letter-spacing`  | 不适用 | 不适用 |                                                              |
| `text-align`      | 不适用 | 不适用 |                                                              |
| `text-decoration` | 不适用 | 不适用 |                                                              |
| `text-indent`     | 不适用 | 不适用 |                                                              |
| `text-overflow`   | 不适用 | 不适用 |                                                              |
| `text-shadow`     | 不适用 | 不适用 |                                                              |
| `text-transform`  | 不适用 | 不适用 |                                                              |
| *边框和背景*      |        |        |                                                              |
| `background`      |   否   |   否   | 受支持，但浏览器之间存在太多不一致，因此不可靠。             |
| `border-radius`   |   否   |   否   |                                                              |
| `box-shadow`      |   否   |   否   |                                                              |

### 图像按钮



请参阅`image`输入类型：

| 属性              |   ñ    |   Ť    | 注意              |
| :---------------- | :----: | :----: | :---------------- |
| *CSS盒子模型*     |        |        |                   |
| `width`           |   是   |   是   |                   |
| `height`          |   是   |   是   |                   |
| `border`          |   是   |   是   |                   |
| `margin`          |   是   |   是   |                   |
| `padding`         |   是   |   是   |                   |
| *文字和字体*      |        |        |                   |
| `color`           | 不适用 | 不适用 |                   |
| `font`            | 不适用 | 不适用 |                   |
| `letter-spacing`  | 不适用 | 不适用 |                   |
| `text-align`      | 不适用 | 不适用 |                   |
| `text-decoration` | 不适用 | 不适用 |                   |
| `text-indent`     | 不适用 | 不适用 |                   |
| `text-overflow`   | 不适用 | 不适用 |                   |
| `text-shadow`     | 不适用 | 不适用 |                   |
| `text-transform`  | 不适用 | 不适用 |                   |
| *边框和背景*      |        |        |                   |
| `background`      |   是   |   是   |                   |
| `border-radius`   |  部分  |  部分  | IE9不支持此属性。 |
| `box-shadow`      |  部分  |  部分  | IE9不支持此属性。 |