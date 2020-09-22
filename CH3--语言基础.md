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
