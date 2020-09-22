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

#### 行内代码对比引用文件

虽然可以直接在 HTML 中嵌入 js，但通常最佳实践是尽量用外部引用的方式引入 js 文件。注意，这里没有关于此实践硬性严格要求，关于外部文件的争议以下几点：

- **可维护性** 分散于格式各样 HTML 页面中的 js 代码无疑是存在维护难度的。用一个文件夹管理所用到的 js 以便开发者基于文件目录修改目标代码。
- **缓存** 浏览器通过特定设置缓存所有外部 js 文件，意味着如果你连两个网页使用了相同的文件，文件只需要被下载一次。这意味着更快的页面加载。
- **未来趋势** 使用外部引用文件，就没必要使用 XHTML 或者文件预先声明这类 hack 方法了。引入外部文件的语法，在 HTML 和 XHTML 中也是相同。

值得一提的考虑，当配置外部文件如何被请求时是它们对请求带宽的影响。（？？？这段话我看不明白 One notable consideration when configuring how external files are requested is their implication on request bandwidth. ）使用 SPDY/HTTP2，每个请求的开销从根本上被减少了，因而在像客户端传输脚本作为 js 组件轻量级依赖时是具备优势的。<br>
例如，你的第一个页面可能有下列引用：

```js
<script src="mainA.js"></script>
<script src="component1.js"></script>
<script src="component2.js"></script>
<script src="component3.js"></script>
```

之后的页面加载可能有下面引用：

```js
<script src="mainB.js"></script>
<script src="component3.js"></script>
<script src="component4.js"></script>
<script src="component5.js"></script>
```

在初始化请求时，如果浏览器支持 SPDY/HTTP2，它可以有效的获取同个终端的文件数，接着它会将他们基于预备文件加入到浏览器缓存。从浏览器的角度来看，通过 SPDY/HTTP2 获取这些独立的资源们应该有大致和传输僵化而庞大的 js 参数相似的延迟。<br>
第二个界面请求，因为你将你的应用划分到轻量缓存文件中，一些第二个页面也依赖的组件早已存在了缓存中。<br>
当然，这假定浏览器支持 SPDY/HTTP2，是只对现代浏览器适用的假设。僵化而庞大的数据可能会对老的浏览器更加适配。

#### 文件模式

IE5.5 通过使用文件类型（doctype）切换，引入了文件模式的概念。开始的两个文件模式是怪异模式（使 IE 表现的像 5 版本一样但包含几个非标准的特性）和标准模式（使 IE 表现的更符合规范的方式）。虽然这两种模式渲染内容上最基本的不同点是与 CSS 相关的，这里也有几处和 js 相关的影响。在些影响，我们会在本书进行讨论。<br>
自 IE 开始引入了文件模式的概念起，其他浏览器也跟着进行匹配。随着标准被大家承认，一个被称做几乎标准的模式冉冉升起。那个模式有很多标准模式的特性但并没有那么严格的去限制。最主要的不同是处理图片周围的空格的情景（最常见的就是在表格里使用图片时）<br>
怪异模式在所有浏览器中都是通过不写文件开头的 doctype 来切换。这考虑到不好的实践因为怪异模式很难在跨浏览器中做兼容，除非 hack 手段否则真实浏览器的等级一致性是做不到的。<br>
下面任意 doctype 被使用就意味着打开标准模式：

```js
<!-- HTML 4.01 Strict -->
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">

<!-- XHTML 1.0 Strict -->
<!DOCTYPE html PUBLIC
"-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">

<!-- HTML5 -->
<!DOCTYPE html>
```

几乎标准模式（almost standards）都通过过渡性的，框架性的 doctypes 来转换，如下：

```js
<!-- HTML 4.01 Transitional --> <!DOCTYPE HTML PUBLIC
"-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">

<!-- HTML 4.01 Frameset -->
<!DOCTYPE HTML PUBLIC
"-//W3C//DTD HTML 4.01 Frameset//EN" "http://www.w3.org/TR/html4/frameset.dtd">

<!-- XHTML 1.0 Transitional -->
<!DOCTYPE html PUBLIC
"-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">


<!-- XHTML 1.0 Frameset -->
<!DOCTYPE html PUBLIC
"-//W3C//DTD XHTML 1.0 Frameset//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-frameset.dtd">
```

因为几乎标准模式和标准模式很接近，很少有区别。人们讨论“标准模式”可能是在说它们之一，关于文件模式的探测也不做区别。这本书中标准模式这个词就代表了除了怪异模式只外的所有模式。**_(20-09-18)_**

#### <NOSCRIPT\> 元素

早期浏览器都很关注的一个问题是当页面不支持 script 时我们如何做到优雅的降级。最后，noscript 元素被创造出来提供给不支持 js 的浏览器。虽然事实上 100% 的现代浏览器都支持 js，这个标签对于特别声明不支持 js 的浏览器依旧是有用的。<br>
noscript 元素可以包裹任何 HTML 元素，除了 script 元素外的所有文件 body 标签内的元素。包裹于 noscript 元素内的内容仅在以下两种情况展示：

- 浏览器不支持 js 脚本
- 浏览器的 js 支持被关闭了
  如果其中任意一个条件达成，noscript 标签中的元素就会别渲染。在其他情况下，浏览器不会渲染 noscript 中的内容。<br>
  这里有个简单例子：

```js
<!DOCTYPE html> <html>
<head>
<title>Example HTML Page</title>
<script ""defer="defer" src="example1.js"></script>
<script ""defer="defer" src="example2.js"></script>
</head>
<body>
<noscript>
<p>This page requires a JavaScript-enabled browser.</p>
</noscript>
</body>
</html>
```

这个例子中，当浏览器不支持 script 时就会把消息展示给用户。对于支持 js 的浏览器，这些消息永远不会被展示，即是它依旧是页面的一部分内容。

#### 总结

js 通过 script 元素插入到 HTML 界面。这个元素可以用来把 js 嵌入到 HTML 界面中，通过结束标记的方式进行行内使用，或者引入存在于外部文件的 js。以下为关键几点：

- 为了引入 js 文件，src 属性必须设置被引入文件的 URL，这个文件可以是在和页面相同的服务器上也可以是另一个完全不同的域名。
- 所有的 script 标签都是按出现在页面中顺序被解释执行的。只要没有使用 derfer 或 async 属性，script 标签中代码必须在上一个 script 标签中代码被执行后才能开始执行。
- 对于非延迟 js，浏览器必须完全解释执行 script 标签内的代码之后才能继续渲染剩余界面。因此，script 标签往往是在页面结束位置使用，在主内容之后且恰好在 body 的结束标签前。
- 你可以延迟执行 js 到文件渲染完毕。defer 标记的脚本会按照他们出现的顺序执行。
- 你可以通过使用 async 属性，声明一个 js 文件既不需要等其他 js 执行，也不阻塞文件渲染。异步脚本不会按照他们出现在文件中位置来顺序执行。

通过使用 noscript 元素，你可以在浏览器不支持 js 时指定显示任意你想展示的内容。

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
