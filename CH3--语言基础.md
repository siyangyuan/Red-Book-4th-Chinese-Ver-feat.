## 3 语言基础

### 语法

ECMAScript 的语法很大程度的借鉴了 C 和其它 类似 C 的语言如 Java 和 Perl。熟悉这类语言的开发者应该能简单的学习并使用起这门语法上自由的语言。

#### 区分大小写

首先需要理解的基本概念是，任何东西都是区分大小写的；变量，方法名，操作符全是大小写敏感的，意味着一个名为 test 的变量是与一个名为 Test 的变量不同的。类似的，typof 不能作为一个方法名因为它是一个关键字；然而，typeof 是一个很合理的方法名。

#### 标识符

标识符是变量的，方法的，属性的或者方法参数的名称。标识符可以是一个或多个字符以下列规则组成：

- 开头的字符必须是字母（letters），一个下划线（\_）或者一个刀了符 (\$)。
- 其它字符可以是字母，下划线刀了符或者数字。
  字母是可能包含 SACII 或者 Unicode 字符的比如 À 和 Æ，尽管这种方式是不推荐的。<br>
  通常，ECMAScript 的标识符使用驼峰命名法，意味着第一个字母需要小写，后续的新增词汇需要通过一个大写字符来分开，如下：

```js
firstSecond;
myCar;
doSomethingImportant;
```

尽管这没有被严格要求，但遵从下述规则的形式被认为是构建 ECMAScript 方法和对象的最佳实践。

> 关键词，保留字，true，false 和 null 不能被用作标识符。“Keywords and Reserved Words” 章节有描述。

#### 注释

ECMAScript 对于单行和块都使用 C 风格的注释。如下：

```js
// single line comment
/* This is a multi-line comment */
```

#### 严格模式

ECMAScript5 引入了严格模式的概念。严格模式是 js 的一个不同的词法分析和执行模式，区别于 ECMAScript3 演说的不确定行为和不安全动作的错误抛出。通过在文件首部加入如下代码，为脚本设置安全模式：

```js
“use strict”
```

虽然这个看起来更像一个字符串而不是一个变量，它是一个编译附注告知 js 引擎使用严格模式运行。特地选中这样的语法来兼容 ECMAScript3 的语法。<br>
你也可以通过在方法体顶部引入编译附注，来指定某个方法运行在严格模式下。

```js
function doSomething() {
  "use strict";
  // function body
}
```

严格模式改变了 js 很多部分的执行，如此，本书会指出严格模式的很多特点。所有现代浏览器都支持严格模式。

#### 语句

js 中的语句是以分号结束的，通过忽略分号让解析器决定语句在哪结束，如下方示例：

```js
let sum = a + b; // valid even without a semicolon - not recommended
let diff = a - b; // valid - preferred
```

虽然分号不是必须的，但你还是应该写上。引入引号避免了忽略错误，比如不结束你输入的语句，允许开发者通过移除多余的空格来压缩 js 代码。引入分号也在特定情况下提升了性能因为解析器通过插入分号来修正语法错误。<br>
多条语句可以通过 C 语法风格合并成一个代码块，开始于一个左大括号以右大括号结束：

```js
if (test) {
  test = false;
  console.log(test);
}
```

控制语句，比如 if，仅当执行多条语句时需要代码块。然而，就算只有一条语句需要被执行，我们也会在控制语句使用代码块。

```js
// valid, but error-prone and should be avoided
if (test) console.log(test);
// preferred
if (test) {
  console.log(test);
}
```

在控制语句中使用代码块使内容更加清晰，减少了修改代码时产生错误的可能。**_(20-09-22)_**

### 关键字和保留字

ECMA-262 描述了一组有特殊用处的关键字，比如声明了开始或结束语句或执行特定指令的保留字。根据规则，关键字是被保留的，无法被用作标志符或属性名称。ECMA-262 第六版的完整关键字列表如下：

```js
break case catch class const continue debugger default delete
do
else export extends finally for function if import
in typeof instanceof var new void return while super with switch yield this
throw try
```

规范也提到了一批不能用来命名标识符和属性的未来保留字。就算这些保留字在现有语言中没有任何特殊用途。他们是未来关键字的保留字。<br>
ECMA-262 第六版未来保留字：

```js
Always reserved:
enum

Reserved in strict mode:
implements interface let
package public protected static private

Reserved in module code:
await
```

这些关键字还没有被用作标识符但还可以被用来命名。一般来说，最好避免使用无论是关键字还是保留字来做命名，这样确保了兼容过去和未来的 ECMAScript 版本。

### 变量

ECMAScript 的变量是无类型的，意味着一个变量可以持有任何类型的数据。每个变量只是一个简单的为值准备的具名占位符。这里有三个关键字可以用来声明一个变量：var ，在所有版本的 ECMAScript 版本可用，let 和 const 都是在 ECMASCript6 中引入的。

### var 关键字

使用 var 操作符（var 是一个关键字）后面跟着一个变量名（一个操作符，如先前描述的），来定义一个变量：

```js
var message;
```

这段代码定义了一个名为 message 的变量，它可以持有任何类型的值。（未初始化，它持有一个特定值 undeifne，下一节会详细讨论。）ECMAScript 实现了变量初始化，所以可以定义变量的同时给他赋值，如下例：

```js
var message = "hi";
```

这里。message 被定义并持有一个 string 值 ‘hi’。做这样的初始化不会让变量被定义成 string 类型；只是简单的给这个变量分配了一个值。有可能不只改变变量存储的值，也改变了它的类型，如下：

```js
var message = "hi";
message = 100; // legal, but not recommended
```

在这个例子中，message 被定义并持有一个 string 值‘hi’，然后重写入一个熟知类值 100.尽管我们不建议改变一个变量持有值的数据类型，但在 SCMAScript 中这是完全生效的。<br>

#### var 声明范围

使用 var 定义变量时，使它作为所处方法的本地变量是很重要的。例如，在方法里定义变量意味着变量将在方法退出时立即销毁，如下：

```js
function test() {
  var message = "hi"; // local variable
}
test();
console.log(message); // error!
```

这里， message 变量在 functon 中使用 var 定义。方法名为 test（），创建了变量并赋值。之后马上，变量就别销毁了所以这个例子的最后一行引发了一个错误。然而，可以通过忽略 var 操作符来定义一个全局变量，如下：

```js
function test() {
  message = "hi"; // global variable
}
test();
console.log(message); // "hi"
```

通过移除 var 操作符号，message 变成了全局变量。一旦 test（） 方法被调用，变量就会被定义切变成外部可访问的。

> **注意** 尽管可以通过忽律 var 来定义全局变量，到这种做法是不推荐的。在本地定义全局变量是难以维护的并且这会导致误解因为你很难在有意忽略 var 的场景下直接理解代码含义。严格模式会抛出一个 ReferenceError 当你给一个未声明的变量赋值时。

如果你需要定义多个变量，你可以通过逗号区分开各个变量来只用一个语句做到：

```js
var message = "hi",
  found = false,
  age = 29;
```

三个变量就被定义且初始化了。因为 SCMAScript 是无类型的，可以用一个语句初始化不同类型的变量。尽管插入换行和缩进比纳凉是非必须的，但这样增加了可读性。<br>
在严格模式下，你不能给变量命名为 eval 或者 arhuments。这样做将会导致语法错误。

#### var 变量提升

使用 var 时，下面代码是可运行的应为变量声明使用--关键字是提升到方法范围顶部的：

```js
function foo() {
  console.log(age);
  var age = 26;
}
foo(); // undefined
```

这不会报错，因为 ECMAScript 运行时对它做了这样的技术处理：

```js
function foo() {
  var age;
  console.log(age);
  age = 26;
}
foo(); // undefined
```

这就是“变量提升（hoisting）”，解释器会把所有变量声明拉到它范围的顶部。它也允许你去冗余的定义变量，这不会引起什么问题：

```js
function foo() {
  var age = 16;
  var age = 26;
  var age = 36;
  console.log(age);
}
foo(); // 36
```

### let 声明

let 操作符和 var 很相似，但也有一些重要的不同点。最值得一提的是，let 为块作用域，但 var 是方法作用域。

```js
if (true) {
  var name = "Matt";
  console.log(name);
}
console.log(name);
if (true) {
  let age = 26;
  console.log(age);
}
console.log(age);
// Matt // Matt
// 26
// ReferenceError: age is not defined
```

这里，age 变量是无法从 if 域外部访问的，因为它的作用范围不包括块的外部。块作用域是方法作用域的严格子集，所以任何对 var 生效的的范围限制也会对 let 声明生效。<br>
let 声明不允许在一个域内冗余的定义变量。这样做会有如下结果：

```js
var name;
var name;
let age;
let age; // SyntaxError; identifier 'age' has already been declared
```

当然，js 引擎会追踪用作变量声明的标志符及它们定义所在的域，所以嵌套使用相同的标识符会如你所想一般不引发错误，因为没有重复定义产生。**_(20-09-23)_**

```js
var name = "Nicholas";
console.log(name); // 'Nicholas' if (true) {
var name = "Matt";
console.log(name); // 'Matt' }
let age = 30;
console.log(age); // 30 if (true) {
let age = 26;
console.log(age); // 26 }
```
