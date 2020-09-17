## 2 html 里的 js

#### <SCRIPT\> 元素

在 html 中插入 js 的基本方法是通过 <script\> 元素。这个标签是由网景创造并在网景浏览器 2 中首次实现。它之后就被加入了 html 的正式标准。这里列举 <script\> 元素的六个属性：<br>

- async 可选的。表明脚本应该马上开始下载，但不能阻塞页面的其他动作：比如下载资源或者等待脚本加载。只对外部脚本文件生效。
- charset 可选的。代码的字符集通过 src 属性指定。这个属性很少用到，因为大多数浏览器不接受它的值。
- crossorgin 可选的。为关联请求配置 CORS 设置。默认情况下。CORS 不会被用到。crossorgin=“anonymous” 为没有认证要求类的请求配置。crossorgin=“use——credentials”将会打开认证选项，意味这外部请求需要包含认证信息。
- defer 可选的。表明脚本的执行被安全的延后到文件内容解析完毕并展示事件后。只对外部脚本生效。IE7 和 早期版本也对行内脚本生效。
- integrity 可选的。允许通过一个密钥来检查得到的数据资源来进行下级资源完好性校验。如果收到的资源的签名与这个配置项的规定不符，页面将会报错脚本不会被执行。这个用来保证 CND 不会服务恶意请求参数。
- language 废弃。过去用来表明 js 版本。
- src 可选的。表明可执行的外部代码文件。
- type 可选的。取代了 language 属性。表明改代码块中 js 的文件类型。通常，这个值一般是 “text/javascript”,然而，“text/script”和“text/ecmascript”都被废弃了。js 文件一般被当作“application/x-javascript”文件类型处理，尽管在 type 属性里这样设置可能会导致这段脚本被忽视。其他在非 IE 浏览器生效的配置为“application/jacascript”和“application/ecmascript”。如果值是 module 的话，代码将被当作 ES6 模块来处理，也只有在这样设置之后 import 和 export 关键字才会生效。<br><br>

有两种使用 script 元素的方式：直接将 js 代码嵌入到页面中。或者从一个外部文件引入 js 文件。<br>
行内的例子如下：

<script>
function sayHi() {
console.log("Hi!");
}
</script>

包含在 script 标签中的 script 代码是自上而下解释执行的。在这个例子中，一个方法被定义且存储在解释器环境中。剩下的页面内容的加载，显示，只有在 script 标签中的代码执行后，才能继续执行。<br>
当使用行内 js 时，注意你的代码中不能有 “</script\>” 这样的字符串。例如下面代码就会在加载过程中报错：<br><br>
<script\>
function sayScript() {
console.log("</script\>");
}
</script\><br><br>
因为行内 js 的词法分析过程中浏览器发现字符串“</script\>”就会认为它是 js 的结束标签。这个问题可以通过避免“/”字符来简单回避。(转义字符 \ )**_(20-09-14)_**<br>
为了从外部引用 js 我们必须用到 src 属性。src 的值是一个 URL 链接链向包含 js 代码的文件，就像这样：<br>

<script src="example.js"></script>

在这个例子中，一个叫 example.js 的外部文件被引用到界面中。文件本身只需要包含将要出现在开闭 script 标签中的代码即可。执行行内 js 代码过程中，解释外部文件时页面构建会停止。（下载文件也会消耗一些时间）。在 XHTNL 文件中，你可以省略结束标签，就如同下例一样：

<script src="example.js"/>
这个语法不应改在 HTML 文件中使用，因为在 HTML 中它是不可用的无法被某些浏览器正确执行，尤其是 IE。

> **_注释_** 通常外部 js 文件会有一个.js 的扩展名。这并不是必要的，因为浏览器不会检查引入的 js 文件的扩展名。这个特点让服务端动态生成 js 代码的设计有了实现的可能性，或是像 React 的 JSX，TS 等 js 的扩展语言能在浏览器内进行 js 语言转换。请注意，然而，服务器经常用文件扩展名来决定应该返回的正确媒体类型（MIME type）。如果你不用 .js 扩展名，复查一下你的服务器是否返回了你正确的媒体类型。

注意，使用了 src 属性的的 script 标签内不要在写入 js 代码。如果你两者都写了的话，js 文件会被下载执行，行内的 js 代码会被忽略。<br>
script 元素可以从外部域名引入 js 文件是它最强大也是最受争议的能力。就像 img 元素一样， 一个内部 HTML 中的 script 元素的 src 属性设置成全路径外部 URl，技能引用改为加，例子如下：
<script src="http://www.somewhere.com/afile.js"></script>

当浏览器去解析这个资源时，它会针对 src 中的具体路径发起 GET 请求，去获取资源——通常是 js 文件。这个初始化请求在浏览器跨域约束下，但是人任何 js 的返回和执行依旧在跨域约束下。当然，请求依旧需要遵循父页面的 HTTP/HTTPS 协议。<br>
外部服务器的代码将会被加载并解释执行，就如同它是引用它界面的一部分。这个能力允许你用各种服务器的 js 支持起你的网页，如果必须的话。不管怎样，在引用你无法控制的服务器的 js 时需要格外小心。恶意程序可以在任何时间替换你的目标文件。当从不同的域引用 js 文件时，请保证你是该域的拥有者，或者这个域是可信的。script 标签的 intergrity 属性给了你防御这类情况的工具。然而，只有部分浏览器支持这个属性。<br>
无论 js 代码是如何引入的， script 元素都是按照他们出现在页面中顺序来执行的（在 derfer 和 async 属性未设置的情况下。第一个 script 元素必须在第二个开始执行被完全执行，第二个必须在第三个之前，如此以往。**_(20-09-15)_**<br>

#### 标签摆放位置

一般情况下，全部的 script 元素被放到一个页面中的 head 元素里，如同下例：

<!DOCTYPE html>
<html>
<head>
<title>Example HTML Page</title>
<script src="example1.js"></script>
<script src="example2.js"></script>
</head>
<body>
</body>
</html>
这样规范格式的主要目的是保证外部引用，包括 CSS 文件和 JS 文件在同一个区域内。然而，在头部引用所有 js 文件意味着在渲染界面前必须下载，语法分析，解释执行完这些 js（渲染过程发生在浏览器发现第一个 body 标签开始）。对于引用了很多 js 代码的页面，这会在页面渲染中引发一个明显的延迟，这段时间内浏览器会完全空白。因此，现代 web 应用，一般在 body 中内容之后引入 js 。<br>
通过这样的方式，页面在 js 代码执行前就被浏览器渲染完毕了。其结果对用户体验的提示就是能感知到页面变快了，因为空白网页的时间减少了。<br>

#### 延迟脚本（deferred script）

HTML 4.01 中为 script 元素定义了名为 defer 的属性。defer 的目的是表明这个脚本在执行时不会改变页面结构。严格来说，脚本可以在整个页面语义分析结束后被安全的执行。在 script 元素中设置 defer 属性向浏览器表明脚本的需要立即开始**下载但是执行需要被延后**。

<script defer src="example1.js"></script>

即是这个 script 元素被放在文件的 head 中，它也会等到浏览器识别到 </html\> 结束标签才会被执行。**HTML 5 的规范中阐述 js 脚本会按照出现顺序执行，所以第一个 defer 标记的脚本会在第二个 defer 标记脚本前执行，他们都会在 DOMCotentLoaded 事件之前执行。实际上，可是，defer 脚本不会一直按顺序执行，也不一定是在 DCL 之前执行，所以可能的话你最好还是只使用一整个来引用比较好。**<br>
之前提到过，defer 属性只支持外部 js 文件。这是在 H5 中规定的，所以支持 H5 实现的浏览器将会在**设置行内代码后忽略 defer 属性。** IE 4-7 是老版本的表现，8 及以上版本支持 H5.<br>
这个属性的指出是从 IE4，火狐 3.5，sarfari5 和 chrome7 开始的。其他浏览器都选择简单的忽略这个属性，把它当成普通的 js 来执行。因为这个原因，把有 defer 标签的 js 脚本放到页面底部是最好的做法。<br>

> **注意** 对于 XHTML 文件来说这样写这个属性 defer=“defer”

#### 异步脚本（asynchromous scripts）

H5 为 script 标签引入了 async 属性。async 属性和 derfer 相似的改变了脚本执行的方式。在支持外部引用脚本和告知浏览器立即下载方面它和 defer 也是相似的。不同于 defer 的是，**async 标记的脚本不会保证按出现顺序执行。**<br>
后面的 js 可能会在，前面的 js 前执行，所以这些 js 间不要存在依赖关系。标记为 async 的原因为表明页面不需要加载前等待某脚本的下载执行，也有另一脚本也不需要等待。因此，建议异步脚本不要执行一些修改 dom 的动作。<br>
异步脚本**严格的在 LOAD 事件前执行或在 DCL 前后执行**。火狐 3.6，safari5 和 Chrome7 支持异步脚本。使用异步脚本意味着你遵守默认规定不会在其中使用 document.write 方法——同时优秀的 web 工程实践也代表你不会在其他任何地方用到这个方法。**_(20-09-16)_**

> **注意** 对于 XHTML 文件来说这样写这个属性 async=“async”

#### 动态脚本加载

你可以用不限于 script 标签的方式去引用资源。因为 js 是可以调用 DOM API 的，你也可以添加 script 元素，来加载指定的资源。可以通过创建 script 元素然后把他们添加到 DOM 的方式达成：

```js
let script = document.createElement("script");
script.src = "gibberish.js";
document.head.appendChild(script);
```

当然，这个请求直到 HTMLElement 元素被添加到 DOM 时才会被执行。默认的，这种方式创建的脚本是带有 async 属性的。这可能会造成问题，然而，就像所有浏览器都支持 createElement 但不是都支持 async 脚本请求。因此，为了统一动态链接的加载，你可以特别注明标签为异步的：

```js
let script = document.createElement("script");
script.src = "gibberish.js";
script.async = false;
document.head.appendChild(script);
```

这种方式加载的资源不会被浏览器的预加载器识别到。这将会严重的破坏脚本们在资源加载队列中的优先级。决定于你的应用是如何工作的及如何使用的，它会严重的破坏性能。为了告知预加载器这个动态资源的存在，你可以在文件头部进行声明。

```js
<link rel="subresource" href="gibberish.js">
```

#### 在 XHTML 中的变化

Extensible HyperText Markup Language, or XHTML，是一个 HTML 作为 XML 应用的重构，不像 HTML 里，type 属性是可以在使用 script 时省略的，在 XHTML 里，script 元素需要你注明 type 属性为 text/javascript。<br>
编码规则在 XHTML 中比 HTML 要严格的多，影响了 script 元素的行内写法。下面代码会在 HTML 中生效，但不会在 XHTML 中生效：

```js
<script type="text/javascript">
function compare(a,b){
  if(a < b){
    console.log('a is less than b')
  }
}
</script>
```

在 HTML 中，script 元素有特别的规则声明了它包含的 JS 的词法解析方式；在 XHTML 中这些特殊规则不会生效。这以为着表述中的小于符号（<）被解释为标签的开始，会引起一个 < 符号后不能是空格的语法错误。<br>
这里有两种改正语法错误的方式，一种是替换所有 < 符号为它的 HTML 数据（&lt）。结果代码如下：

```js
<script type="text/javascript">
function compare(a,b){
  if(a &lt b){
    console.log('a is less than b')
  }
}
</script>
```

现在这个代码可以在 XHTML 中运行了，但是，这个有一些不易读懂。幸运的是我们还有另一种方法。<br>
第二种在 XHTML 中运行的方法是，把 js 代码转换为 CDATA 部分。在 XHTML （XML）中，CDATA 部分表明，在页面的词法分析中忽略这一部分自由规范的代码。这能让你使用各种字符，包括小于符号，并不会引发语法错误。书写方式如下：

```js
<script type="text/javascript">
<![CDATA[
function compare(a,b){
  if(a < b){
    console.log('a is less than b')
  }
}
]]>
</script>
```

这解决了 XHTML 规范浏览器中的问题。然而，还有很多浏览器不遵循 XHTML 规范也不支持 CDATA 片段。为了在这类浏览器工作， CDATA 部分必须被注释掉。

```js
<script type="text/javascript">
//<![CDATA[
function compare(a,b){
  if(a < b){
    console.log('a is less than b')
  }
}
//]]>
</script>
```

这种形式在所有现代浏览器中都可以工作。虽然有一些 hack ，但它作为 XHTMl 是生效的也为 pre-XHTML 浏览器做了优雅的降级。

> **注意** XHTML 模式是通过标注页面的 MIME type 为 “application/xhtml+xml”。并不是所有的正式浏览器都支持这个设置功能。

#### 废弃语法

从 1995 年网景发布了网景 2 浏览器，所有浏览器都是用 js 作为它们的默认开发语言。type 属性被作为标志 script 内容的 MIME type，但 MIME type 在跨浏览器方面没有被标准化。即是浏览器默认识别为 js，在某些情况下一些无效的或无法识别的 MIME type 的相关代码就会被跳过执行。因此，除非你在用 XHTML 或者 script 标签请求或包裹了 非 js 代码，最佳实践是完全不使用 type 属性。<br>
当 script 元素最先被引入，它就标志这与传统 HTML 页面的语义分析的背离。特殊的规则需要被应用到这个特殊的模块上，这为不支持 js 的浏览器造成了问题（最值得一提的是 Mosaic）。不支持的浏览器会直接将 js 标签的内容输出到页面上，有效的运行页面显示。<br>
网景再开发了 Mosaic 产出了一个解决方案不支持的浏览器会把嵌入的 js 代码隐藏。最终方案是将 HTML 中的 js 代码备注起来：

```js
<script><!--
function sayHi(){ console.log("Hi!");
}
//--></script>
```

用这种形式，像 Mosaic 这样的浏览器将会安全的忽略 script 标签内的内容，支持 js 的浏览器回去寻找这样的结构识别出这里有必要的 js 内容需要被执行。<br>
虽然这个形式依旧被所有浏览器正确识别和解释执行，它不再是必须的也不应该再使用。在 XHTML 模式下，它也会导致脚本被忽略因为它在一个有效的 XML 内容中。**_(20-09-17)_**
