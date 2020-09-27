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