现在说明了OOJS的大多数细节，本文介绍了如何创建从其“父”类继承特征的“子”对象类（构造函数）。另外，我们为您在何时何地可能使用OOJS提供了一些建议，并介绍了如何使用现代ECMAScript语法处理类。

| 先决条件： | 基本的计算机知识，对HTML和CSS的基本理解，对JavaScript的基础知识（请参阅“ [第一步和[构建模块”）和OOJS的基础知识（请参见[“对象简介”）。 |
| :--------- | ------------------------------------------------------------ |
| 目的：     | 了解如何在JavaScript中实现继承。                             |

## 原型继承

到目前为止，我们已经看到了一些继承作用-我们已经看到了原型链如何工作，以及成员如何沿链向上继承。但这主要涉及内置的浏览器功能。我们如何在JavaScript中创建从另一个对象继承的对象？

让我们用一个具体的例子探讨如何做到这一点。

## 入门

首先，使自己成为我们的[oojs-class-inheritance-start.html](https://github.com/mdn/learning-area/blob/master/javascript/oojs/advanced/oojs-class-inheritance-start.html)文件的本地副本（也[可以实时运行](http://mdn.github.io/learning-area/javascript/oojs/advanced/oojs-class-inheritance-start.html)）。在这里，您会发现`Person()`我们在模块中一直使用的相同构造函数示例，只是略有不同-我们仅在构造函数内部定义了属性：

```js
function Person(first, last, age, gender, interests) {
  this.name = {
    first,
    last
  };
  this.age = age;
  this.gender = gender;
  this.interests = interests;
};
```

该方法*都*定义在构造函数的原型。例如：

```js
Person.prototype.greeting = function() {
  alert('Hi! I\'m ' + this.name.first + '.');
};
```

**注意**：在源代码中，您还将看到`bio()`和`farewell()`定义的方法。稍后，您将看到其他构造函数如何继承它们。

假设我们要创建一个`Teacher`类，就像在最初的面向对象的定义中描述的那样，该类继承的所有成员`Person`，但还包括：

1. 一个新的属性，其中`subject`将包含老师教授的主题。
2. 一种更新的`greeting()`方法，听起来比标准`greeting()`方法更正式-更适合老师在学校里对一些学生讲话。

## 定义Teacher（）构造函数

我们需要做的第一件事是创建一个`Teacher()`构造函数-在现有代码下面添加以下内容：

```js
function Teacher(first, last, age, gender, interests, subject) {
  Person.call(this, first, last, age, gender, interests);

  this.subject = subject;
}
```

这在很多方面看起来类似于Person构造函数，但是这里有一个我们之前从未见过的奇怪东西- `call()`函数。该函数基本上允许您调用在其他地方定义的函数，但是在当前上下文中。第一个参数指定`this`运行该函数时要使用的值，其他参数是调用该函数时应传递给该函数的那些参数。

我们希望`Teacher()`构造函数采用`Person()`与其继承的构造函数相同的参数，因此我们将它们全部指定为`call()`调用中的参数。

构造函数中的最后一行仅定义了`subject`教师将要拥有的新属性，而普通人则没有。

注意，我们可以简单地做到这一点：

```js
function Teacher(first, last, age, gender, interests, subject) {
  this.name = {
    first,
    last
  };
  this.age = age;
  this.gender = gender;
  this.interests = interests;
  this.subject = subject;
}
```

但这只是重新定义属性，而不是从继承它们`Person()`，因此它违背了我们试图做的事情。它还需要更多的代码行。

### 从没有参数的构造函数继承



请注意，如果您要从其继承的构造函数不从参数获取其属性值，则无需在中将其指定为其他参数`call()`。因此，例如，如果您有一个非常简单的东西，例如：

```js
function Brick() {
  this.width = 10;
  this.height = 20;
}
```

您可以这样做来继承`width`和`height`属性（当然还有下面介绍的其他步骤）：

```js
function BlueGlassBrick() {
  Brick.call(this);

  this.opacity = 0.5;
  this.color = 'blue';
}
```

请注意，我们仅`this`在内部指定`call()`-不需要其他参数，因为我们不会从父级继承通过参数设置的任何属性。

## 设置Teacher（）的原型和构造方法参考

到目前为止一切都很好，但是我们有一个问题。我们定义了一个新的构造函数，它具有一个`prototype`属性，默认情况下，该属性仅包含一个对象，该对象带有对构造函数本身的引用。它不包含Person构造函数的`prototype`属性的方法。要查看此信息，请`Object.getOwnPropertyNames(Teacher.prototype)`在文本输入字段或JavaScript控制台中输入。然后再次输入，替换`Teacher`为`Person`。新的构造函数也不会*继承*这些方法。看到这一点，比较器的输出`Person.prototype.greeting`和`Teacher.prototype.greeting`。我们需要`Teacher()`继承在`Person()`原型上定义的方法。那么我们该怎么做呢？

1. 在您之前添加的内容下面添加以下行：

   ```js
   Teacher.prototype = Object.create(Person.prototype);
   ```

   在这里，我们的朋友`create()`再次来营救。在这种情况下，我们将使用它创建一个新对象并将其值设为

   ```
   Teacher.prototype。新对象具有
   ```

   ```
Person.prototype
   ```
   
   其原型，因此将在需要时继承上可用的所有方法

   ```
Person.prototype。
   ```
   
2. 我们需要继续做一件事。在添加最后一行之后，`Teacher.``prototype`的`constructor`属性现在等于`Person()`，因为我们只是设置`Teacher.prototype`为引用一个对象，该对象继承自`Person.prototype`！尝试保存代码，在浏览器中加载页面，然后进入`Teacher.prototype.constructor`控制台进行验证。

3. 这可能会成为一个问题，因此我们需要设置此权限。您可以通过返回源代码并在底部添加以下行来实现：

   ```js
   Object.defineProperty(Teacher.prototype, 'constructor', { 
       value: Teacher, 
       enumerable: false, // so that it does not appear in 'for in' loop
       writable: true });
   ```

4. 现在，如果您保存并刷新，则输入`Teacher.prototype.constructor`应`Teacher()`根据需要返回，此外，我们现在继承自`Person()`！

## 给Teacher（）一个新的greeting（）函数

为了完成我们的代码，我们需要`greeting()`在`Teacher()`构造函数上定义一个新函数。

最简单的方法是在`Teacher()`的原型上进行定义-在代码底部添加以下内容：

```js
Teacher.prototype.greeting = function() {
  let prefix;

  if (this.gender === 'male' || this.gender === 'Male' || this.gender === 'm' || this.gender === 'M') {
    prefix = 'Mr.';
  } else if (this.gender === 'female' || this.gender === 'Female' || this.gender === 'f' || this.gender === 'F') {
    prefix = 'Ms.';
  } else {
    prefix = 'Mx.';
  }

  alert('Hello. My name is ' + prefix + ' ' + this.name.last + ', and I teach ' + this.subject + '.');
};
```

这会警告老师的问候语，并使用条件语句计算出来，该问候语还使用了适合其性别的名称前缀。

## 试试这个例子

现在，您已经输入了所有代码，尝试`Teacher()`通过将以下内容放在JavaScript底部（或您选择的类似内容）来创建对象实例：

```js
let teacher1 = new Teacher('Dave', 'Griffiths', 31, 'male', ['football', 'cookery'], 'mathematics');
```

现在保存并刷新，然后尝试访问新`teacher1`对象的属性和方法，例如：

```js
teacher1.name.first;
teacher1.interests[0];
teacher1.bio();
teacher1.subject;
teacher1.greeting();
teacher1.farewell();
```

这些都应该正常工作。第1、2、3和6行的查询访问从通用`Person()`构造函数（类）继承的成员。第4行的查询访问仅在更专门的`Teacher()`构造函数（类）上可用的成员。第5行的查询将访问从继承的成员`Person()`，但`Teacher()`拥有自己的成员具有相同名称的事实除外，因此查询将访问该成员。

**注意**：如果您无法正常使用它，请将您的代码与我们的[最终版本](https://github.com/mdn/learning-area/blob/master/javascript/oojs/advanced/oojs-class-inheritance-finished.html)进行比较（也[可以实时](http://mdn.github.io/learning-area/javascript/oojs/advanced/oojs-class-inheritance-finished.html)查看代码）。

我们这里介绍的技术不是在JavaScript中创建继承类的唯一方法，但是它可以正常工作，并且为您提供了有关如何在JavaScript中实现继承的好主意。

您可能还对检查一些新的[ECMAScript功能感兴趣，这些功能使我们能够在JavaScript中更干净地进行继承（请参阅[Classs）。我们在此不介绍这些内容，因为它们尚未在各种浏览器中得到广泛支持。早在IE9或更早的版本中，我们就支持了本文中讨论的所有其他代码构造，并且还有一些方法可以实现更早的支持。

一种常见的方法是使用JavaScript库-大多数流行的选项都具有一组简单的功能，可用于更轻松，更快速地进行继承。[的CoffeeScript](http://coffeescript.org/#classes)例如提供`class`，`extends`等等。

## 进一步的练习

在我们的[OOP理论部分](1/en-US/docs/Learn/JavaScript/Objects/Object-oriented_JS#Object-oriented_programming_from_10000_meters)，我们还包括一个`Student`类作为概念，它继承了的所有功能`Person`，并且具有一种不同于问候的非正式`greeting()`方法。看一下该部分学生的问候语，然后尝试实现自己的构造函数，该构造函数继承的所有功能并实现不同的功能。`Person``Teacher``Student()``Person()``greeting()`

## 对象成员摘要

总而言之，您需要担心四种类型的属性/方法：

1. 那些在构造函数内部定义的对象实例。这些都是很容易发现的-在您自己的自定义代码中，它们是在构造函数中使用`this.x = x`类型线定义的成员。在内置的浏览器代码中，它们是仅对象实例可用的成员（通常是通过使用`new`关键字调用构造函数，例如`let myInstance = new myConstructor()`）创建的。
2. 直接在构造函数本身上定义的那些，仅在构造函数上可用。这些通常仅在内置浏览器对象上可用，并且可以通过直接链接到构造函数（*而不是*实例）上来识别。例如，`Object.keys()`。这些也称为**静态属性/方法**。
3. 那些在构造函数原型上定义的对象，它们被所有实例和继承的对象类所继承。这些成员包括在构造函数的`prototype`属性上定义的任何成员，例如`myConstructor.prototype.x()`。
4. 对象实例上可用的对象可以是在实例化构造函数时创建的对象（例如`var teacher1 = new Teacher( name = 'Chris' );`，如上所示）（例如，然后是`teacher1.name`），也可以是对象常量（`let teacher1 = { name = 'Chris' }`然后是`teacher1.name`）。

如果您不确定哪个是哪个，就不必担心它-您仍在学习中，熟悉会伴随实践。

## ECMAScript 2015类

ECMAScript 2015将[类语法引入JavaScript，作为一种使用更简单，更简洁的语法来编写可重用类的方法，该语法与C ++或Java中的类更相似。在本节中，我们将把Person和Teacher的示例从原型继承转换为类，向您展示它是如何完成的。

**注意**：这种现代的编写类方法在所有现代的浏览器中均受支持，但是如果您在需要支持不支持该语法的浏览器的项目中工作，那么仍然值得了解基础原型继承。 ）。

让我们看一下Person类实例的重写版本：

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

  greeting() {
    console.log(`Hi! I'm ${this.name.first}`);
  };

  farewell() {
    console.log(`${this.name.first} has left the building. Bye for now!`);
  };
}
```

在[类的语句表明我们正在创造一个新的类。在此块内，我们定义该类的所有功能：

- 该`constructor()`方法定义了代表我们`Person`类的构造函数。
- `greeting()`和`farewell()`是类方法。您想要与该类关联的任何方法都在构造函数之后在其内部定义。在此示例中，我们使用[模板文字而不是字符串连接来使代码更易于阅读。

现在，我们可以使用[`new`运算符实例化对象实例，就像之前一样：

```js
let han = new Person('Han', 'Solo', 25, 'male', ['Smuggling']);
han.greeting();
// Hi! I'm Han

let leia = new Person('Leia', 'Organa', 19, 'female', ['Government']);
leia.farewell();
// Leia has left the building. Bye for now
```



### 类语法的继承



上面我们创建了一个代表人的类。它们具有所有人共有的一系列属性。在本节中，我们将创建专门的`Teacher`类，使其`Person`使用现代类语法继承。这称为创建子类或子类。

要创建子类，我们可以使用[extends关键字告诉JavaScript我们要基于其类的类，

```js
class Teacher extends Person {
  constructor(subject, grade) {
    this.subject = subject;
    this.grade = grade;
  }
}
```

但是有一点收获。

不像老派构造函数[`new`运营商做的初始化`this`到新分配的对象，这不是自动初始化由定义的类[扩展关键词，即子类。

因此，运行上面的代码将产生错误：

```js
Uncaught ReferenceError: Must call super constructor in derived class before
accessing 'this' or returning from derived constructor
```

对于子类，对`this`新分配的对象的初始化始终取决于父类的构造函数，即您要从中扩展的类的构造函数。

在这里，我们扩展`Person`类- `Teacher`子类是类的扩展`Person`。因此，对于`Teacher`，`this`初始化由`Person`构造函数完成。

要调用父构造函数，我们必须使用[`super()`operator，如下所示：

```js
class Teacher extends Person {
  constructor(subject, grade) { 
    super(); // Now 'this' is initialized by calling the parent constructor.
    this.subject = subject;   
    this.grade = grade; 
  }
}
```

如果没有从父类继承的属性，则没有子类是没有意义的。
很好的是，[`super()`运算符也接受父构造函数的参数。

回顾我们的`Person`构造函数，我们可以看到它的构造函数方法中包含以下代码块：

```js
 constructor(first, last, age, gender, interests) { 
   this.name = {   
     first,   
     last   
   }; 
   this.age = age; 
   this.gender = gender;   
   this.interests = interests; 
} 
```

由于`super()`运算符实际上是父类构造函数，因此将其传递给类构造函数的必要参数`Parent`也将在我们的子类中初始化父类属性，从而继承它：

```js
class Teacher extends Person {
  constructor(first, last, age, gender, interests, subject, grade) {
    super(first, last, age, gender, interests);

    // subject and grade are specific to Teacher
    this.subject = subject;
    this.grade = grade;
  }
}
```

现在，当我们实例化`Teacher`对象实例时，我们可以调用在两者上定义的方法和属性`Teacher`，`Person`就像我们期望的那样：

```js
let snape = new Teacher('Severus', 'Snape', 58, 'male', ['Potions'], 'Dark arts', 5);
snape.greeting(); // Hi! I'm Severus.
snape.farewell(); // Severus has left the building. Bye for now.
snape.age // 58
snape.subject; // Dark arts
```

就像我们对教师所做的那样，我们可以创建的其他子类，`Person`以使它们更加专业化，而无需修改基类。



## 吸气剂和二传手

有时候，我们想更改我们创建的类中的属性值，或者我们不知道属性的最终值是多少。使用`Teacher`示例，我们可能不知道老师在创建这些科目之前会教什么科目，否则他们的科目可能会在每学期之间改变。

我们可以使用getter和setter处理此类情况。

让我们通过getter和setter增强教师班级。该课程的开始时间与我们上次查看该课程的时间相同。

获取器和设置器成对工作。一个getter返回变量的当前值，其对应的setter将变量的值更改为它定义的值。

修改后的`Teacher`类如下所示：

```js
class Teacher extends Person {
  constructor(first, last, age, gender, interests, subject, grade) {
    super(first, last, age, gender, interests);
    // subject and grade are specific to Teacher
    this._subject = subject;
    this.grade = grade;
  }

  get subject() {
    return this._subject;
  }

  set subject(newSubject) {
    this._subject = newSubject;
  }
}
```

在上面的类中，我们有一个用于该`subject`属性的获取器和设置器。我们使用**`_`** 创建一个单独的值来存储我们的name属性。不使用此约定，每次调用get或set都会出错。在此刻：

- 为了显示对象`_subject`属性的当前值，`snape`我们可以使用`snape.subject`getter方法。
- 要将新值分配给`_subject`属性，我们可以使用`snape.subject="new value"`setter方法。

下面的示例显示了正在使用的两个功能：

```js
// Check the default value
console.log(snape.subject) // Returns "Dark arts"

// Change the value
snape.subject = "Balloon animals" // Sets _subject to "Balloon animals"

// Check it again and see if it matches the new value
console.log(snape.subject) // Returns "Balloon animals"
```

**注意：**有时，getter和setter可能非常有用，例如，当您希望每次请求或设置属性时都运行一些代码时。但是，对于简单的情况，没有getter或setter的普通属性访问就可以了。

## 什么时候在JavaScript中使用继承？

特别是在上一篇文章之后，您可能会想到“这很复杂”。好吧，你是对的。原型和继承表示JavaScript的一些最复杂方面，但是JavaScript的强大功能和灵活性来自其对象结构和继承，值得了解它的工作方式。

在某种程度上，您一直都在使用继承。每当您使用Web API的各种功能，或在字符串，数组等上调用的内置浏览器对象上定义的方法/属性时，都在隐式使用继承。

就在您自己的代码中使用继承而言，您可能不会经常使用它，尤其是在小型项目中尤其如此。仅在不需要对象时才使用对象和继承是浪费时间。但是随着您的代码库越来越大，您更有可能发现它的需求。如果您发现自己开始创建许多具有相似功能的对象，那么创建一个通用对象类型以包含所有共享功能并在更专门的对象类型中继承这些功能既方便又有用。

**注意**：由于JavaScript的工作方式，原型链等的原因，对象之间的功能共享通常称为**委托**。专用对象将功能委托给通用对象类型。

使用继承时，建议您不要拥有太多级别的继承，并仔细跟踪定义方法和属性的位置。可以开始编写临时修改内置浏览器对象原型的代码，但是除非有充分的理由，否则不要这样做。当您尝试调试此类代码时，太多的继承会导致无休止的混乱和无尽的痛苦。

最终，对象只是具有其特定角色和优点的另一种代码重用形式，例如函数或循环。如果您发现自己创建了一堆相关的变量和函数，并希望将它们一起跟踪并整齐地打包，则对象是个好主意。当您要将一组数据从一个地方传递到另一个地方时，对象也非常有用。无需使用构造函数或继承就可以实现这两件事。如果您只需要一个对象的单个实例，那么最好只使用一个对象文字，并且您当然不需要继承。

