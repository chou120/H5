在[CSS中，选择器用于定位我们要设置样式的网页上的[HTML元素。有多种CSS选择器可供选择，在选择要设置样式的元素时可以实现细粒度的精度。在本文及其子文章中，我们将详细介绍各种类型，并了解它们的工作原理。

| 先决条件： | 基本的计算机知识，[已安装的基本软件]( 1
| :--------- | ------------------------------------------------------------ |
| 目的：     | 了解CSS选择器的详细工作方式。                                |

## 什么是选择器？

您已经遇到选择器。CSS选择器是CSS规则的第一部分。它是元素和其他术语的一种模式，它告诉浏览器应选择哪些HTML元素以将规则内的CSS属性值应用于它们。选择器选择的一个或多个元素称为选择器的*主题*。

![突出显示h1的一些代码。](https://mdn.mozillademos.org/files/16550/selector.png)

在较早的文章中，您遇到了一些不同的选择器，并且了解到有一些选择器以不同的方式定位文档，例如，通过选择诸如的元素`h1`或诸如的类的选择器`.special`。

在CSS中，选择器是在CSS选择器规范中定义的；像CSS的其他任何部分一样，它们需要在浏览器中获得支持才能正常工作。您将遇到的大多数选择器是在[Level 3 Selectors规范](https://www.w3.org/TR/selectors-3/)（这是一个成熟的规范）中定义的，因此您会发现这些选择器具有出色的浏览器支持。

## 选择器列表

如果您有多个使用同一CSS的东西，那么可以将单个选择器组合到一个*选择器列表中，*以便将规则应用于所有单个选择器。例如，如果我有一个相同的CSS `h1`以及一个类`.special`，则可以将其编写为两个单独的规则。

```
h1 { 
  color: blue; 
} 

.special { 
  color: blue; 
} 
```

通过在它们之间添加逗号，我也可以将它们组合到选择器列表中。

```
h1, .special { 
  color: blue; 
} 
```

空格在逗号之前或之后有效。如果每个选择符都在换行符上，您可能还会发现它们更具可读性。

```
h1, 
.special {
  color: blue; 
} 
```

在下面的实时示例中，尝试结合使用两个具有相同声明的选择器。合并后，视觉显示应相同。

```
span {
    background-color: yellow;
}

strong {
    color: rebeccapurple;
}

em {
    color: rebeccapurple;
}
    
```

 ```
<h1>Type selectors</h1>
<p>Veggies es bonus vobis, proinde vos postulo essum magis <span>kohlrabi welsh onion</span> daikon amaranth tatsoi tomatillo
    melon azuki bean garlic.</p>

<p>Gumbo beet greens corn soko <strong>endive</strong> gumbo gourd. Parsley shallot courgette tatsoi pea sprouts fava bean collard
    greens dandelion okra wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini.</p>

<p>Turnip greens yarrow ricebean rutabaga <em>endive cauliflower</em> sea lettuce kohlrabi amaranth water spinach avocado
    daikon napa cabbage asparagus winter purslane kale. Celery potato scallion desert raisin horseradish spinach
</p>
    
 ```



以这种方式对选择器进行分组时，如果任何选择器无效，则整个规则将被忽略。

在下面的示例中，无效的类选择器规则将被忽略，而`h1`仍然将设置样式。

```
h1 { 
  color: blue; 
} 

..special { 
  color: blue; 
} 
```

但是，当组合在一起时，`h1`由于整个规则都被认为是无效的，所以也不会对类和类进行样式设置。

```
h1, ..special { 
  color: blue; 
} 
```

## 选择器类型

选择器有几种不同的分组，知道您可能需要哪种选择器类型将有助于您找到适合该工作的工具。在本文的子文章中，我们将更详细地介绍不同的选择器组。

### 类型，类别和ID选择器



该组包括针对HTML元素（例如）的选择器`<h1>`。

```css
h1 { }
```

它还包括针对一个类的选择器：

```css
.box { }
```

或者，ID：

```css
#unique { }
```

### 属性选择器



这组选择器为您提供了基于元素上某个属性的存在来选择元素的不同方法：

```css
a[title] { }
```

甚至根据具有特定值的属性的存在进行选择：

```css
a[href="https://example.com"] { }
```

### 伪类和伪元素



这组选择器包括伪类，它们为元素的某些状态设置样式。`:hover`例如，伪类仅在将鼠标指针悬停在元素上时才选择它：

```css
a:hover { }
```

它还包括伪元素，这些伪元素选择元素的某个部分而不是元素本身。例如，`::first-line`始终选择元素内的文本的第一行（``在以下情况下为a），就像将a ``环绕在第一格式化的行上然后选中一样。

```css
p::first-line { }
```

### 组合器



最后一组选择器将其他选择器组合在一起，以定位文档中的元素。

```css
article > p { }
```



## 选择器参考表

下表概述了可使用的选择器，并提供了指向本指南页面的链接，这些链接将向您展示如何使用每种选择器。我还为每个选择器都提供了指向MDN页面的链接，您可以在其中检查浏览器支持信息。当您稍后需要在材料中查找选择器时，或者在尝试使用CSS时，可以将其用作参考。

| 选择器                                                       | 例                  | 学习CSS教程                                                  |
| :----------------------------------------------------------- | :------------------ | :----------------------------------------------------------- |
| [类型选择器]( /Type_selectors) | `h1 { }`            | [类型选择器](1/en-US/docs/user:chrisdavidmills/CSS_Learn/CSS_Selectors/Type_Class_and_ID_Selectors#Type_selectors) |
| [通用选择器]( /Universal_selectors) | `* { }`             | [通用选择器](1/en-US/docs/user:chrisdavidmills/CSS_Learn/CSS_Selectors/Type_Class_and_ID_Selectors#The_universal_selector) |
| [类选择器]( /Class_selectors) | `.box { }`          | [类选择器](1/en-US/docs/user:chrisdavidmills/CSS_Learn/CSS_Selectors/Type_Class_and_ID_Selectors#Class_selectors) |
| [ID选择器]( /ID_selectors) | `#unique { }`       | [ID选择器](1/en-US/docs/user:chrisdavidmills/CSS_Learn/CSS_Selectors/Type_Class_and_ID_Selectors#ID_Selectors) |
| [属性选择器]( /Attribute_selectors) | `a[title] { }`      | [属性选择器](1/en-US/docs/User:chrisdavidmills/CSS_Learn/CSS_Selectors/Attribute_selectors) |
| [伪类选择器]( /Pseudo-classes) | `p:first-child { }` | [伪类](1/en-US/docs/User:chrisdavidmills/CSS_Learn/CSS_Selectors/Pseuso-classes_and_Pseudo-elements#What_is_a_pseudo-class) |
| [伪元素选择器]( /Pseudo-elements) | `p::first-line { }` | [伪元素](1/en-US/docs/User:chrisdavidmills/CSS_Learn/CSS_Selectors/Pseuso-classes_and_Pseudo-elements#What_is_a_pseudo-element) |
| [后代组合器]( /Descendant_combinator) | `article p`         | [后代组合器](1/en-US/docs/User:chrisdavidmills/CSS_Learn/CSS_Selectors/Combinators#Descendant_Selector) |
| [儿童组合器]( /Child_combinator) | `article > p`       | [儿童组合器](1/en-US/docs/User:chrisdavidmills/CSS_Learn/CSS_Selectors/Combinators#Child_combinator) |
| [相邻的同级组合器]( /Adjacent_sibling_combinator) | `h1 + p`            | [相邻兄弟姐妹](1/en-US/docs/User:chrisdavidmills/CSS_Learn/CSS_Selectors/Combinators#Adjacent_sibling) |
| [通用同级组合器]( /General_sibling_combinator) | `h1 ~ p`            | [一般同级](1/en-US/docs/User:chrisdavidmills/CSS_Learn/CSS_Selectors/Combinators#General_sibling) |