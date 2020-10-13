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

重复声明的报错是与顺序无关的，也与是否混用 var let 无关。不同类型的关键字不会声明不同类型的变量——它们只是指定了变量的关联作用域。

```js
var name;
let name; // SyntaxError
let age;
var age; // SyntaxError
```

#### 现世的死域（temporal dead zone）

另一区分 let 和 var 的重要行为是 let 声明不会产生变量提升现象：

```js
// name is hoisted
console.log(name); // undefined
var name = "Matt";
// age is not hoisted
console.log(age); // ReferenceError: age is not defined
let age = 26;
```

当解析代码时，js 引擎会察觉到 let 声明在作用范围的稍后部分会出现，但这些变量无法在真正的声明出现前被引用。声明前的代码执行被称为“现世的死域（temporal dead zone）”，任何试图引用这类变量的行为都会抛出错误 ReferenceError。

#### 全局声明

不像 var 关键字，当使用 let 在全局上下文声明变量时，变量不会像 var 一样关联到 widow 对象。

```js
var name = "Matt";
console.log(window.name); // 'Matt'
let age = 26;
console.log(window.age); // undefined
```

然而，let 声明还是会在全局作用域内发生，将和页面的生命周期共存。因此，你必须保证你的页面不会进行重复声明以免产生语法错误。

#### 条件声明

当使用 var 声明变量时，因为声明被提升，js 引擎会开心的在作用域顶部将多个冗余声明合并成一个。因为 let 声明被域限制到域内，不可能检查 let 变量是否被定义了，并且有条件的定义它如果它没被定义的话。

```js
<script>
var name = 'Nicholas'; let age = 26;
</script>
<script>
// Suppose this script is unsure about what has already been declared in the page.
// It will assume variables have not been declared.
var name = 'Matt';
// No problems here, since this will be handled as a single hoisted declaration.
// There is no need to check if it was previously declared.
let age = 36;
// This will throw an error when 'age' has already been declared.
</script>
```

使用 try/catch 语句或者 typeof 操作符不是解决办法，如下在条件结构体范围内使用 let 声明会导致变量限制在域内。

```js
<script>
let name = 'Nicholas';
let age = 36;
</script>
<script>
// Suppose this script is unsure about what has already been declared in the page.
// It will assume variables have not been declared.
if (typeof name !== 'undefined') {
  let name;
}
// 'name' is restricted to the if {} block scope,
// so this assignment will act as a global assignment
name = 'Matt';
try (age) {
// If age is not declared, this will throw an error
}
catch(error) {
let age;
}
// 'age' is restricted to the catch {} block scope, // so this assignment will act as a global assignment
age = 26;
</script>
```

因此，你无法使用条件声明的方式来处理这个 ES6 的声明关键字。

> **注意** 无法使用在条件声明使用 let 关键字是一件好事，因为在你的代码中条件声明不是一个好的方式。它让程序流更难理解。如果你发现自己接近了这种方式，这是一个很好的机会去找一个更好的写代码方式。

#### 在循环声明中的 let 关键字

在 let 出现前，for 循环的定义引入一个迭代变量，这个变量会泄漏到循环体之外；

```js
for (var i = 0; i < 5; ++i) {
  // do loop things
}
console.log(i); // 5
```

当转向使用 let 声明后，迭代比阿亮将会限制到循环体内：

```js
for (let i = 0; i < 5; ++i) {
  // do loop things
}
console.log(i); // ReferenceError: i is not defined
```

使用 var，一个常见的问题的是唯一的定义和迭代值的修改：

```js
for (var i = 0; i < 5; ++i) {
  setTimeout(() => console.log(i), 0);
}
// You might expect this to console.log 0, 1, 2, 3, 4
// It will actually console.log 5, 5, 5, 5, 5
```

这种现象发生是因为循环退出但迭代值依然被赋值引发了，循环在 5 的时候退出。当 timeout 晚些被执行时，它们引用了相同的变量，因此 console.log 打出了最终值。<br>
当使用 let 去声明循环迭代器时，在背后 js 引擎实际在每个循环中声明都声明了一个新的迭代值。每次 setTimeout 引用的都是独立的实例，然后 console 会打出预期的值：循环迭代执行时迭代变量的值。

```js
for (let i = 0; i < 5; ++i) {
  setTimeout(() => console.log(i), 0);
}
// console.logs 0, 1, 2, 3, 4
```

这种每个迭代中声明的行为对任何方式的循环都是生效的，包括 for in 和 for of 循环。**_(20-09-24)_**

### const 声明

const 和 let 相近，但有一点重要的不同——它必须被赋值初始化,并且声明后不能再被定义。试图修改一个 const 值会抛出一个运行时错误。

```js
const age = 26;
age = 36; // TypeError: assignment to a constant
// const still disallows redundant declaration
const name = "Matt";
const name = "Nicholas"; // SyntaxError
// const is still scoped to blocks
const name = "Matt";
if (true) {
  const name = "Nicholas";
}
console.log(name); // Matt
```

const 声明被强制指向它引用的变量。如果 const 变量引用了一个 object，它不会严格禁止修改该对象内的属性。

```js
const person = {};
person.name = "Matt"; // ok
```

尽管 js 引擎会为在循环中的 let 迭代变量创建新的实例，尽管 cosnt 的表现和 let 相近，你不能用 const 去声明循环迭代器：

```js
for (const i = 0; i < 10; ++i) {} // TypeError: assignment to constant variable
```

然而，如果你想声明一个不可修改的 for 循环变量，const 是可用的--恰好地因为每次迭代都会创建一个新变量。这尤其与 for-of 和 for-in 循环相关。

```js
let i = 0;
for (const j = 7; i < 5; ++i) {
  console.log(j);
}
// 7, 7, 7, 7, 7
for (const key in { a: 1, b: 2 }) {
  console.log(key);
}
// a, b
for (const value of [1, 2, 3, 4, 5]) {
  console.log(value);
}
// 1, 2, 3, 4, 5
```

### 声明风格和最佳实践

在 ES6 中引入了 let 和 const 客观的更好的规范了这门语言，应对日益提升的精准作用域声明及语义学。var 声明的诡异方式让整个 js 社区一期纠结掉了很多年的头发。在新关键字的启发下，这里有一些可以提升代码质量的通用模式出现。

#### 不要用 var

有了 let 和 const，大多数开发者不再需要在编码中使用到 var。感谢对变量作用域的细心管理，只允许使用 let 和 const 的模式保证了代码的质量，本地化声明，和 const 的正确性。

#### 相较 let 最好用 const

使用 const 声明允许浏览器运行时强制变量不可变，此外静态代码分析器会强制禁止违规的重新赋值操作。因此，很多开发者觉得这是有好处的，默认的，会将一个值声明为 const 除非他们知道将在未来某个点需要改变这个变量。允许开发者更清晰的知道这个值是不可变的，并可以快速检测到预期外的行为，比如代码试图执行预期之外的重新赋值操作。

### 数据类型
ECMAScript 中存在六种简单类型（也叫基础类型）：Undefined，Null，Boolean，Number，String，Symbol（翻译者的话：书里还没有bigint）。Symbol 是 ES6 中新引入的。也有一种复杂类型叫做 Object，是一个无序的键值队。因为无法在 ECMAScript 中定义你自己的数据类型，所有值可以被认为是这其中类型中的一个。只有其中数据类型可能看起太少了不能完全表达数据类型；然而，ECMAScript 有动态外观使得每个单独的数据类型表现得像多种。
#### typeof 操作符
因为 ECMAScript 是无类型的，也就需要一个方法去决定给定变量的数据类型。typeof 操作符提供了这个信息。在一个值上使用 typeof 操作符号会返回下面字符串之一
```shell
"undefined" if the value is undefined
"boolean" if the value is a Boolean
"string" if the value is a string
"number" if the value is a number
"object" if the value is an object (other than a function) or null 
"function" if the value is a function
"symbol" if the value is a Symbol
```
typeof 操作符是这样调用的：
```js
let message = "some string";
console.log(typeof message); // "string" 
console.log(typeof(message)); // "string"
console.log(typeof 95); // "number"
```
在这个例子中，不管是一个变量（message）还是一个被传入 typeof 操作符的字面意义的数字。注意，因为 typeof 是一个操作符而不是一个方法，所以不许要括号（尽管这样是可用的）。<br>
注意这里有几个地方 typeof 返回了技术上正确的但看起来奇怪的值。调用 typeof null 返回的值为 “object”，特殊值 null 被认为是一个空对象的引用。
> **注意** 技术上，在 ECMAScript 中方法被认为是对象，而不是其它类型。然而，它们确实有一下特别的属性，是的有必要通过 typeof 操作符来区分它们。***(20-09-25)***

### undefined 类型
undefined 类型只有一个值，就是特殊值 undefined。当一个变量使用 var 或 let 声明但缺没有初始化，它会如下被设置为 undefined 值：
```js
let message;
console.log(message == undefined); // true
```
在这个例子中，变量 message 被声明了，却没有进行初始化。当和字面的值 undefined 对比时，得到的结果是相等的。这个例子和下面是相同的：
```js
let message = undefined;
console.log(message == undefined); // true
```
这里的变量 message 被指定初始化为 undefined。这是非必须的，因为默认，任何没有初始化的值会被赋上 undefined。
> **注意** 一般来说，你不应该将一个值特地设置为 undefined。字面的 undefined 值只要是用来进行比较而且是直到 ECMA-262第三版 才被加进来的，用来规范区分一个空对象指针（null）和未初始化的变量。

注意，一个变量的值为 undefined 和一个完全未定义的变量是不同的，如下：
```js
let message; // this variable is declared but has a value of undefined
// make sure this variable isn't declared // let age
console.log(message); // "undefined"
console.log(age); // causes an error
```
在这个例子中，第一个 log 会实际打印出变量 message，为 undefined。在第二个 log 中，一个未声明的变量 age 被出入到 console.log（）方法中，会因为没有声明变量引发一个错误。只有一个操作符可以用来操作未声明的变量：你可以在它上面调用 typeof (也可以调用 delete 不会引发错误，但这没有用处且在严格模式下会引发错误,**翻译者，自己试了下Reflect.deleteProperty() 是会报错的**)。<br>
typeof 操作符当操作一个未初始化变量时返回 undefined，但是它也会返回 undefined 当操作一个未声明的变量时，这有些令人困扰。看看这个例子：
```js
let message; // this variable is declared but has a value of undefined
// make sure this variable isn't declared
// let age
console.log(typeof message); // "undefined" 
console.log(typeof age); // "undefined
```
在两个例子中，在变量上调用 typeof 会返回 undefined。逻辑上，这说的通因为没有一个操作符可以区分两个变量，尽管他们技术上是很不同的。
> **注意** 尽管未初始化的变量会自动被赋值为 undefined，还是推荐你去初始化变量。这样，当 typeof 返回 undefined 的时候，你就会简单的明白，是这个值没有被声明，而不是没有被简单的初始化。

undefined 这个值是带有’否定‘意义的。由此，你可以在需要使用它时进行简单的检查。牢记，有很多其它值也是‘否定’意义的，所以小心预测当你需要特别检测 undefined 值而不是简单的‘否定’意义值。
```js
let message; // this variable is declared but has a value of undefined
// 'age' is not declared
if (message) {
// This block will not execute
}
if (!message) {
// This block will execute
}
if (age) {
// This will throw an error
}
```
### null 类型
null 类型第二个只有一个值的数据类型：特殊值 null。逻辑上，null 是一个空的对象指针，所以调用 typeof 会返回“object”当在下面例子传入一个 null 时：
```js
let car = null;
console.log(typeof car); // "object"
```
定义一个之后用来持有对象的变量时，建议初始为 null 而不是其它任何值。这样，你可以特定检查 null 值来看一个变量是否晚些时候被 object 赋值，比如在这个例子中：
```js
if (car != null) {
// do something with car
}
```
undefined 是 null 的衍生物，所以 ECMA-262 将他们定义为表面上相等：
```js
console.log(null == undefined); // true
console.log(null === undefined); // false 翻译者 在node10 下实验
```
在 null 和 undefined 间使用相等操作符（==），返回 true，注意这个操作符会为了比较目的转换它所比较的变量（操作值）。<br>
即使 null 和 undefined 是相近的，他们的用处大不相同。如之前提到的，你不应该特意将一个值设置为 undefined，但这对 null 不适用。任何时候一个变量被需要但不可用时，null 就派上了用场。这帮助你保证了 null 的范例——它是一个空对象指针，且将它和 undefined 作区别。<br>
null 也是否定意义的；由此可以在用到一个值时简单的进行检查。注意，和undefined 注意的一样，不翻译了。
```js
let message = null; 
let age;
if (message) { // This block
}
if (!message) { // This block
}
if (age) {
// This block
}
if (!age) {
// This block
}
```
### boolean 类型
boolean 类型 ECMAScript 中最常用的数据类型之一，它只有两个字面值：true 和 false。这两值是与数字值相区别的，所以 true 和 1 是不相等的，flase 和 0 也是不相等的。为 Boolean 变量赋值方法如下：
```js
let found = true; 
let lost = false;
```
注意 boolean 字面值是大小写敏感的，所以 True 和 False（和其它大小写混合的写法）是有效的变量标识符，而不是 boolean 值。<br>
虽然 boolean 只有两个字面值，所有类型的值在 ECMAScript 都有一个对应的 boolean 值。(省了几句废话)转换方法为：
```js
let message = "Hello world!";
let messageAsBoolean = Boolean(message);
```
在这个例子中 message 被转换为一个 boolean 值且存到了 messageAsBoolean 变量中。Boolean() 转换方法在任何数据类型的值上调用且总是返回一个 boolean 值。转换规则与被转换值的类型及实际值相关。下表列出了数据类型和它们的特殊转换。

| 数据类型 | 转换为true的值 | 转换为false的值 |
|------|------------|------------|
| Boolean  | true          | false         |
| String  | 任何非空值        | “”空字符串        |
| Objct  | 任何对象       | null      |
| Number  | 非零数（包含无穷大 infinity）       | 0，NAN       |
| Undefined  | n/a      | undefined      |

这些转换是需要重点理解的，因为流程控制，比如 if 语句，自动的体现了 boolean 转换，如下：
```js
let message = "Hello world!";
if (message) {
console.log("Value is true");
}
```
在这个例子中，log 会被打出来，因为 string 类型的 message 自动转化为它的 boolean 对应值（true）。因为这个自动转换，理解你在流程控制中的变量是很重要的。错误的使用一个对象去替代 boolean 值会彻底的改变你的程序流程。***(20-09-27)***

### number 类型
也许 ECMAScript 中最有趣的数据类型就是 number，使用 IEEE-754 规范来代表整型和浮点值（也在某些语言中称为双精度值）。为了支持数字多样的类型，有几种不同的数字字面格式。<br>
最基本的是十进制整型，可以直接键入，如下：
```js
let intNum = 55; // integer
```
整型也可以被表示为八进制或者十六进制，第一个数字必须是0后面跟随一串八进制数字（0到7）。如果在这字面量中有一个数字被监测到超过了这个范围，开头的0就会被忽略数字会被当作十进制处理，如下面例子：
```js
let octalNum1 = 070;// octal for 56
let octalNum2 = 079;// invalid octal - interpreted as 79 
let octalNum3 = 08;// invalid octal - interpreted as 8
```
八进制字面量在严格模式是不生效的并且会导致 js 引擎抛出一个语法错误。<br>
为了创建一个十六进制数，你必须指定开头两个字节 0x（大小写敏感），后续跟着任意十六进制数（0到9，A 到 F）。字符可以是大写或者小写。
```js
let hexNum1 = 0xA; // hexadecimal for 10
let hexNum2 = 0x1f; // hexadecimal for 31
```
用八或者十六进制创建的数字会在所有数学运算中被当作十进制数来处理。
> **注意** 因为 js 中数字的存储方式，它实际上可能会有大于0或者小于0.大于0和小于0是被认为在任何情况下都是对等的，但在这里做一个澄清。

#### 浮点值
为了定义一个浮点值，你必须至少引入一个小数点且至少后面跟上一个数字。尽管小数点前没有必须要有一个整型，但我们推荐这样做。这里有一些例子：
```js
let floatNum1 = 1.1;
let floatNum2 = 0.1;
let floatNum3 = .1; // valid, but not recommended
```
因为存储浮点值消耗了存储整型两倍的空间，ECMAScript 总会寻找将值转换为整型的方法。当小数点后面没有数字时，这个值就会变成整型。而且，如果值是一个整数（如1.0），它也会被转换为整型，如下例：
```js
let floatNum1 = 1.; // missing digit after decimal - interpreted as integer 1
let floatNum2 = 10.0; // whole number - interpreted as integer 10
```
对于每一个大的或者非常小的数字。浮点值可以用科学计数法（e-notation）表达式。他用一个数乘以10的指定次方来表示值。ECMAScript 规范中需要有一个数（整数或者浮点数）后面跟着一个小写或者大写的 E，后面再跟着10的次方。参考如下：
```js
let floatNum = 3.125e7; // equal to 31250000
```
在这个例子中，floatNum 等于 31,250,000 尽管使用科学记数法让它的表达方式变得更精简。这个符号的基本含义是“取 3.125，然后乘上 $10^7$ .”<br>
科学记数法也可以表示很小的数，比如 0.00000000000000003,可以简明的表示为 3e-17.默认的，ECMAScript 转换任何任何小数点后有六个零以上的浮点值为科学记数法形式（如，0.0000003 变成 3e–7）.<br>
浮点数会精确到 17 个小数位但远不及整个数字的算术计算。例如，0.1 + 0.2 得出  0.30000000000000004 而不是 0.3.这些小的约数是的检查浮点数值变得很困难。参考下例：
```js
if (a + b == 0.3) { // avoid! 
console.log("You got 0.3.");
}
```
这里，测试结果表明两个数的和不等于0.3。0.05和0.025之类的是符合逻辑的。但如果是0.1 + 0.2，如我们之前讨论的，这个结果会变成否定。因此不要相信验证浮点数的结果。
> **注意** 理解约数错误是来自 IEEE-754（二进制浮点数算术标准）的浮点计算的影响，并非 ECMAScript 中独有的。其它语言使用了相同规范的话会有相同的问题。

#### 值范围
因为内存限制，并不是自然界所有数都能在 ECMAScript 中被表示。ECMAScript 中能表示的最小数存储在大多数浏览器中国呢 Number.MIN_VALUE 且值为 5e–324；最大值存储在 Number.MAX_VALUE 中且在大多数浏览器中为 1.7976931348623157e+308。如果一个数无法在 ECMAScript 的数字范围内被表示，该数字会自动的获取到特殊值 Infinity。任何无法被表示的负数会表示成  –Infinity，任何无法被表示的整数会表示成 Infinity。<br>
如果一次计算返回整数或者负数的 Infinity，这个值将无法用在任何计算中。因为 Infinity 没有用来计算的数学含义。决定一个值是否为无穷（即，它在最小和最大之间），有一个 isFinite() 方法来验证。仅当参数在最小和最大之间时，这个方法会返回 true，如下例：
```js
let result = Number.MAX_VALUE + Number.MAX_VALUE; 
console.log(isFinite(result)); // false
```
尽管很少会有超过无限大范围的计算，但这是有可能的并且在做很小或很大的数值的计算时因该检测该过程避免出错。
> **注意** 你也可以通过 Number.NEGATIVE _ INFINITY 和 Number.POSITIVE _ INFINITY 来获取正的和负的无穷数。如你预想的，这些属性分别包含了 –Infinity 和 Infinity 值。

#### NaN
有一个叫 NaN 的特殊数字值，AKA 不是个数（Not a Number），引入用来在操作符试图返回一个数字却失败了的场景（预期返回一个错误）。例如，在其语言用 0 分割任意数字一般会引发错误，在代码执行过程中。在 ECMAXcript 中，用 0 分割一个数字会返回 NaN，允许其它操作继续进行。(翻译者没搞懂，dividing any number by 0 typically causes an error，咋分割的啊？)<br>
NaN 有一对独有属性。一，任何操作符触及 NaN 都会返回 NaN（例如：NaN /10），可能会在多步计算中引发问题。二，NaN 不等于任何值，包括 NaN，例如下例：
```js
console.log(NaN == NaN); // false
```
因此，ECMAScript 提供了 isNaN() 方法。这个方法传入单个参数，可以是任何数据类型，去决定一个值是否“不是一个数字”。当一个值被传入 IsNaN（），会尝试将它转为数字。一些非数字的值会直接转化为数字，比如 String “10” 或者一个 Boolean 值。任何无法被转化为数字的值会让方法返回 true，参考下例：
```js
console.log(isNaN(NaN)); // true
console.log(isNaN(10)); // false - 10 is a number
console.log(isNaN("10")); // false - can be converted to number 10
console.log(isNaN("blue"));  // true - cannot be converted to a number
console.log(isNaN(true)); // false - can be converted to number 1
```
本例中测试了五个不同的值。第一个检测了 NaN 本身，显然的返回 true。接下来来那个为数字10和字符串“10”，都返回 false，因为两个的数值值为10.字符串“blue”，然而，无法被转化为数字，所以返回 true。boolean 值 true 可以被转化为 1，所以方法返回 false。
> 尽管一般不会这样做，isNaN() 可以在对象上使用。这种情况，object 的 ValueOf（）方法会先被调用决定返回的值是否能转化为数字。如果不行，toString（）方法聚会被调用且它返回的值也会被检验。这种接续的方式作用在 ECMAScript 构造方法和操作符里，会在后续的 “操作符” 章节细讲。

#### Number 转换
有三个方法可以将非数值值转换为数值值：Number（）转化方法，parseInt（）方法，parseFloat（）方法。第一个方法可以用在任何数据类型上。另外两个方法用来将 string 转为 number。这些方法对于相同的输入有不同的反应。<br>
Number（）方法的转换基于下列规则：
* 当用在 Boolean 值上，true 和 false 分别转化为 1 和 0.
* 当应用于 number 时，值简单的传入然后返回。
* 当应用于 null 时，返回 0.
* 当应用于 undefined 时，返回 NaN。
* 当应用于 strings 时，下列规则会被实行：<br>
1，如果字符串只含有数字字符，在前面加上任意 + 或者 -，它会转化为十进制数，所以 Number（“1”） 变为 1，Number（“011”）变为 11（注意开头的0，被忽略了）。<br>2，如果包含一个有效浮点数，则转化为数字的浮点值（重申，开头的0会被忽略）。<br>3，如果包含一个有效的十六进制值，如：“0xf”，他会被转化为一个值相当的整型15.<br>4，如果是空的，被转化为0.5，如果包含的数不在前四种场景中则转化为 NaN。
* 当应用于 object 时，valueOf（）方法被调用并计入先前描述的规则转换。如果转化结果是 NaN，toString（）方法被调用，接着进入转换 string 的规则。

从各种类型转化为 number 时复杂的，如之前阐述的对 Number（）的规则。这里有些具体例子：
```js
let num1 = Number("Hello world!"); // NaN
let num2 = Number(""); // 0
let num3 = Number("000011"); // 11
let num4 = Number(true); // 1
```
在这个例子中，string “hello world”被转化为 NaN 因为它没有对应的数值值，空的字符串会被转化为0.字符串“00011”被转化为数字11，因为初始化0会被忽略。最后，true 被转化1.***(20-09-28)***
> **注意** 一元操作符 + ，我们会在后续“操作符”章节中，详细讨论，和 Number（） 方法的工作过程相似。

因为 Number（） 方法在转换 string 时的复杂和奇怪现象，parseInt（）方法往往是处理整数时更好的选择。parseInt（）方法会更精确的检验 string 看他是否匹配数字的范式。开头的空格会被忽略直到周到第一个非空格字符。如果开头不是数字，正号，负号，parseInt() 就会返回 NaN，意味着空字符转也会返回 NaN（不像 Number（），返回 0）。如果开头字符是数字，正好，负号，转换就会到达下一个字符直到找到不符合的字符。例如，“1234blue”被转换为“1234”因为blue会被忽略掉。类似的，“22.5”会被转化为22因为小数点不是一个有效的整数字符。<br>
假设开始的第一份字符是数字，parseInt（）方法也能识别不同的整数形式（十进制，8，16）。这意味着当字符串开头是“0x”，它会被转化为16进制整数；如果它以“0”开头跟着数字，他就会识别为一个8进制数。<br>
这里举一些转换例子更好的解释了发生了什么：
```js
let num1 = parseInt("1234blue"); // 1234
let num2 = parseInt("");// NaN
let num3 = parseInt("0xA");// 10 - hexadecimal
let num4 = parseInt(22.5); // 22
let num5 = parseInt("70");// 70 - decimal
let num6 = parseInt("0xf");// 15 - hexadecimal
```
所有不同的数字转化是在追踪过程中是令人困惑的，所以 parseInt（）提供了第二个参数：进制。如果你直到你的值是16进制的，你可以在第二参数传入进制数 16 来保证正确的执行，如下：
```js
let num = parseInt("0xAF", 16); // 175
```
实际上，通过设置16进制参数，你可以不用在开头使用“0x”，
```js
let num1 = parseInt("AF", 16); // 175
let num2 = parseInt("AF"); // NaN
```
在这个例子中，第一个转换会正确进行，但第二个就会失败。不同的是第一个传入了进制数，告诉 parseInt（）它将对一个十六进制数进行转化，第二个发现第一个字符不是数字然后就自动停了下来。<br>
传递进制会完全改变转换结果，参考如下:<br>
翻译者吐槽：这不是著名的面试题吗？？[1, 2, 3].map(parseInt);
```js
let num1 = parseInt("10", 2); // 2 - parsed as binary
let num2 = parseInt("10", 8); // 8 - parsed as octal
let num3 = parseInt("10", 10); // 10 - parsed as decimal
let num4 = parseInt("10", 16); // 16 - parsed as hexadecimal
```
因为进制数允许parseInt（）选择如何转换输入，建议总是传入进制数来避免错误。
> **注意** 大多数时候你都是在换十进制数，所以经常给第二个参数传入10是好的做法。

parseFolat（） 方法和 parseInt（） 的运行规则类似，看开始字符。它也会持续转化直到结束或者遇到浮点数中非法的值。这意味这小数点仅在第一次数显会生效，第二个是非法的，切剩下的字符串会被忽略，导致“22.34.5”被转化为 22.34.<br>
另一个不停是开头的0会被忽略。这个方法会识别我们先前桃林的任意浮点数，就像十进制转换（开始的0总是被忽略）。16进制数会被转化为0.因为 parseInt（）只转化十进制数，没有进制模式。最后一点：如果转换的全是数字（没有小数点或小数点后只有零），该方法会返回一个整型（integer）。(翻译者想到：前面提到浮点占的内存是整型的两倍。)***(20-09-29)***
```js
let num1 = parseFloat("1234blue");// 1234 - integer
let num2 = parseFloat("0xA"); // 0
let num3 = parseFloat("22.5");// 22.5
let num4 = parseFloat("22.34.5");// 22.34
let num5 = parseFloat("0908.5");// 908.5
let num6 = parseFloat("3.125e7");// 31250000
```

### string类型
string 数据表示可一个0或多个的 16-bits unicode 字符序列。字符串可以用双引号（“），单引号（‘），或者 backticks（`）描述，所以如下都是合法的：
```js
let firstName = "John";
let lastName = 'Jacob';
let lastName = `Jingleheimerschmidt`
```
不像一些其它语言，可以使用不同的引号来改变字符串的解释方式，在 ECMAScript 语法中是没有区别的。注意，然而，一个字符串还有和结尾的符号必须是相同的。例如，该例子就会引发一个语法错误：***(20-10-09)***
```js
let firstName = 'Nicholas"; // syntax error - quotes must match
```
#### 字符常量
字符串数据类型包含了几种字符常量来表示不可打印的或其它用处的字符，如下列举：

| 字面字符 | 意义 |
|------|------------|
| \n | 新的一行 |
| \t | tab |
| \b | 退格 |
| \r | 回车 |
| \f | 换页 |
| \\ | 下划线（\） |
| \' | 当引号（'）——当使用的string是被单引号标注的。例如：'he said,\'hi.\' |
| \" | 双引号 |
| \` | 重音符 |
| \xnn | 通过十六进制编码表示字符 nn 为十六进制数 0-F。例如：\x41 等价于 "A" |
| \unnn | 通过十六进制码表示一个 unicode 字符。例如：\u03a3 等价于希腊字符Σ |
这些字符常量可以在string的任何位置被引入，且会被解释为当一的字符，如下：
```js
let text = "sigma: \u03a3.";
```
例子中，变量 test 是长度 9 的字符串尽转译字符长为 6 字符。整个转译序列代表了一个单一字符，所以它是如此计算的。<br>
任何字符串的长度可以使用 length 属性获取，如下：
```js
console.log(text.length); // 9
```
这个属性返回了字符串中 16-bit 字符的数量。
> **注意** 如果一个 string 包含了双类型的字符，length 属性可能就无法确切的返回字符串中字符的数量。这个情况的缓解策略在**基本引用类型**章节中有详细说明。

#### string 的本质
string 在 ECMAScript 中是不可变的（immutable），意味着他们一旦被创建，他们的值就不可变。去改变一个变量持有的字符串值，原有的字符串会被销毁且变量会被另一个包含新值的字符串填充，如下：
```js
let lang = "Java";
lang = lang + "Script";
```
这里变量 lang 被定义包含了字符串“java”。下一行，lang 被重定义去结合了“java”和“Script”，值为“javaScript”。这通过创建一个新的空间为10字符的字符串，然后用“java”和“script”填充了这个字符串变量。程序的最后一步是摧毁原先的字符串“java”和“script”，因为他们都不再被需要。这都是表像之后发生的事件，这也是老版本浏览器(such as pre–1.0 versions of Firefox and Internet Explorer 6.0)在string相关处理很慢的原因。这些低效的部分在这些浏览器的后续版本中有被定为到。<br>
#### 转变为一个字符串
有两种将一个值转变为字符串的方法。第一种是用 toString（） 方法，这是几乎所有值都有的。这个方法的唯一工作是返回目标值的对应字符串值。参考下例：
```js
let age = 11;
let ageAsString = age.toString(); // the string "11"
let found = true;
let foundAsString = found.toString(); // the string "true"
```
toString（）方法在数字，布尔，对象，字符串值上都是可用的。如果值是null或者undefined，这个方法就不可用了。<br>
在大多数例子中，toString（）没有任何参数。然而，当调用者是数值时，toString（）会接收一个参数：输出数字的进制。默认的，toString（）会以十进制输出，但通过穿参可以输出2，8，16或任何合法的进制输出，如下：
```js
let num = 10; 
console.log(num.toString()); // "10" 
console.log(num.toString(2)); // "1010"
console.log(num.toString(8));  // "12"
console.log(num.toString(10));  // "10" 
console.log(num.toString(16));// "a"
```
这个例子展示了toString（）在由进制传入时输出的变化。10 可以输出为任意进制格式的数字。注意默认（未提供参数）和出入进制数10是相同的结果。<br>
如果你无法确认传入值是不是 null 或 undefined，你也用 String（）转化方法，会不管值的类型返回一个字符串。String（）方法符合下列规则：
* 如果值有toString（）方法，它即会被调用（无参数）并返回结果。
* 如果值是 null，“null”被返回。 undefined 同null。
```js
let value1 = 10;
let value2 = true;
let value3 = null;
let value4;
console.log(String(value1));// "10"
console.log(String(value2)); // "true"
console.log(String(value3)); // "null"
console.log(String(value4));// "undefined"
```
此处，四个值被转化为字符串，数字，布尔，null 和 undefined。数字的结果和布尔的结果和toString（）被调用是相同的。因为 toString（）对于 null 和 undefined 是不可用的，String（）方法会简单的返回这些值的字面值。
> **注意** 你可以通过在一个值前面（+）加上空字符串“”来把一个值转化为字符串值（晚些会在**操作符**一章讨论）

#### 模版字符串
在 ECMAScript6 中引入新的可能，用模版字符串定义字符串。不像单引号和双引号对照，模版字符串识别换行字符，可以被定义扩展多行：
```js
let myMultiLineString = 'first line\nsecond line';
let myMultiLineTemplateLiteral = `first line second line`;
console.log(myMultiLineString);
// first line
// second line"
console.log(myMultiLineTemplateLiteral); 
// first line
// second line
console.log(myMultiLineString === myMultiLinetemplateLiteral); // true
```
如名称建议，模版字符串在定义模版时特别有用，如 HTML：
```js
let pageHTML = `
<div>
  <a href="#"> 
   <span>Jake</span>
  </a>
</div>`;
```
因为模版字符串会确实匹配重音符里的空格，当定义他们时特殊的处理会发生。一个正确格式的模版字符串可能会出现不合适的缩进：***(20-10-10)***
```js
// This template literal has 25 spaces following the line return character
let myTemplateLiteral = `first line
second line`;
console.log(myTemplateLiteral.length); // 47

// This template literal begins with a line return character
let secondTemplateLiteral = `
first line
second line`;
console.log(secondTemplateLiteral[0] === '\n'); // true


// This template literal has no unexpected whitespace characters
let thirdTemplateLiteral = `first line
second line`;
console.log(thirdTemplateLiteral[0]);
// first line 
// second line
```
##### 插入文字
模版字符串最有用的特性之一就是对插补文字的支持，允许你在单句不断开的定义中插入值一个或多个值。技术上，模版字符串不是字符串，他们是特殊的 js 语法表达来评估为一个字符串。模版字符串当被定义是就快速的被评估且被转化为字符串实例，且任何插入值会被从当前作用域被引入。<br>
这可以通过使用 ${} 中的 js 表达来完成：
```js
let value = 5;
let exponent = 'second';
// Formerly, interpolation was accomplished as follows:
let interpolatedString =
value + ' to the ' + exponent + ' power is ' + (value * value);
// The same thing accomplished with template literals: let interpolatedTemplateLiteral =
`${ value } to the ${ exponent } power is ${ value * value }`;
console.log(interpolatedString); // 5 to the second power is 25
console.log(interpolatedTemplateLiteral); // 5 to the second power is 25
```
值最后会强制被用 toString（）转化为字符串，但任何 js 表达式都能安全的进行插入。安全的链接模版字符串不需要额外的空格：
```js
console.log(`Hello, ${ `World` }!`); // Hello, World!
```
toString() 被引入来使得表达式转化为string：
```js
let foo = { toString: () => 'World' };
console.log(`Hello, ${ foo }!`); // Hello, World!
```
在插入表达式中使用函数和方法是允许的：
```js
function capitalize(word) {
return `${ word[0].toUpperCase() }${ word.slice(1) }`;
}
console.log(`${ capitalize('hello') }, ${ capitalize('world') }!`); // Hello, World!
```
额外的，模版可以安全的插入变量的之前的值：
```js
let value = ''; function append() {
value = `${value}abc`
console.log(value); }
append(); // abc 
append(); // abcabc 
append(); // abcabcabc
```
##### 模版字符串标签方法
***(20-10-12)***
模版字符串也支持定义标签方法，可以用来定义特定的插入字段行为。标签方法被通过（？ is passed），在模版被插入字段分割的独立的片段后，在表达式被执行后。<br>
一个标签方法被定义为一个常规方法被应用到模版字符串中通过被前置到它之前，如下面代码展示的。标签方法将被通过模版字符串被分割成片：第一个参数是一列字符串组成的数组，剩下的参数为表达式的执行结果。这个方法的返回值是模版字符串的执行结果。<br>
这有个最佳的展示例子：
```js
let a = 6;
let b = 9;
function simpleTag(strings, aValExpression, bValExpression, sumExpression) { 
  console.log(strings);
  console.log(aValExpression);
  console.log(bValExpression);
  console.log(sumExpression);
  return 'foobar';
}
let untaggedResult = `${ a } + ${ b } = ${ a + b }`;
let taggedResult = simpleTag`${ a } + ${ b } = ${ a + b }`
// [ '', ' + ', ' = ', '' ]
// 6
// 9
// 15
console.log(untaggedResult); // "6 + 9 = 15" console.log(taggedResult); // "foobar"
```
因为这里的变量数是可变的，聪明的做法是使用扩展符吧他们组合成单个集合：
```js
let a = 6;
let b = 9;

function simpleTag(strings, ...expressions) {
  console.log(strings);
  for(const expression of expressions) {
      console.log(expression); 
  }
  return 'foobar'; 
}
let taggedResult = simpleTag`${ a } + ${ b } = ${ a + b }`
// [ '', ' + ', ' = ', '' ]
// 6
// 9
// 15
console.log(taggedResult); // "foobar"
```
对于一个有 n 个插入字段的模版字符串，标签方法表达式的参数也会是 n，第一个参数中切割成的字符串片也会总为 n+1。因此，如果你希望“压缩（zip）”字符串并和表达式一起执行返回默认字符串，你可以这样做：
```js
let a = 6; 
let b = 9;
function zipTag(strings, ...expressions) { 
return strings[0] +
    expressions.map((e, i) => `${e}${strings[i + 1]}`) .join('');
}
let untaggedResult = `${ a } + ${ b } = ${ a + b }`;
let taggedResult = zipTag`${ a } + ${ b } = ${ a + b }`;
console.log(untaggedResult); // "6 + 9 = 15" 
console.log(taggedResult); // "6 + 9 = 15"
```
#### 原始字符串
使用模版字符串时可以直接使用原始的字符串而不是转换成实际转化字符，比如换行和 unicode 字符一类。可以通过使用 String.raw 方法来达成，默认下是生效的：
```js
// Unicode demo
// \u00A9 is the copyright symbol
console.log(`\u00A9`); // © 
console.log(String.raw`\u00A9`); // \u00A9


// Newline demo
console.log(`first line\nsecond line`);
// first line
// second line
console.log(String.raw`first line\nsecond line`); // "first line\nsecond line"

// This does not work for actual newline characters: they do not // undergo conversion from their plaintext escaped equivalents
console.log(`first line
second line`);
// first line 
// second line

console.log(String.raw`first line second line`);
// first line
// second line
```
在标签方法中，原始值也做为一个可用属性存在于字符串集合中的没个元素上
```js

function printRaw(strings) {
  console.log('Actual characters:');
  for (const string of strings) {
  	console.log(string); 
  }
  console.log('Escaped characters;'); 
  for (const rawString of strings.raw) {
  	console.log(rawString);
  }
}
printRaw`\u00A9${ 'and' }\n`;
// Actual characters:
// ©
// (newline)
// Escaped characters: // \u00A9
// \n
```
### Symbol类型
ECMAScript 6 中定义了新类型 Symbol 数据类型。Symbol 是原始变量，它的实例是唯一且不可改变的。使用 Symbol 的目的是为对象属性产生唯一的标志符避免碰撞的风险。<br>
尽管他们看起来和私有属性有很多相同点，symbol 不是为了提供私有属性行为（特别的因为 Object API 提供了简单发现 symbol 的方法）。相代替的，symbol 用来创造唯一词以作为特殊值的 key 区别于其它使用 string 为 key 的属性。***(20-10-13)***