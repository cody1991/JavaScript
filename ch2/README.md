`<script>`元素有下列 8 个属性。

- async：可选。表示应该立即开始下载脚本，但不能阻止其他页面动作，比如下载资源或等待其他脚本加载。只对外部脚本文件有效。
- crossorigin：可选。配置相关请求的 CORS（跨源资源共享）设置。默认不使用 CORS。 crossorigin="anonymous"配置文件请求不必设置凭据标志。 crossorigin="use-credentials"设置凭据标志，意味着出站请求会包含凭据。
- defer：可选。 表示脚本可以延迟到文档完全被解析和显示之后再执行。 只对外部脚本文件有效。在 IE7 及更早的版本中，对行内脚本也可以指定这个属性。
- integrity：可选。允许比对接收到的资源和指定的加密签名以验证子资源完整性（ SRI，Subresource Integrity）。如果接收到的资源的签名与这个属性指定的签名不匹配，则页面会报错，脚本不会执行。这个属性可以用于确保内容分发网络（ CDN， Content Delivery Network）不会提供恶意内容。
- type：可选。代替 language，表示代码块中脚本语言的内容类型（也称 MIME 类型）。按照惯例，这个值始终都是"text/javascript"，尽管"text/javascript"和"text/ecmascript"都已经废弃了。 JavaScript 文件的 MIME 类型通常是"application/x-javascript"，不过给 type 属性这个值有可能导致脚本被忽略 。 在非 IE 的浏览器中有效的其他值还有"application/javascript"和"application/ecmascript"。如果这个值是 module，则代码会被当成 ES6 模块，而且只有这时候代码中才能出现 import 和 export 关键字。
- src：可选。表示包含要执行的代码的外部文件。
- language：废弃。最初用于表示代码块中的脚本语言（如"JavaScript"、 "JavaScript 1.2"
  或"VBScript"）。大多数浏览器都会忽略这个属性，不应该再使用它。
- charset：可选。使用 src 属性指定的代码字符集。这个属性很少使用，因为大多数浏览器不在乎它的值。

---

defer 推迟执行脚本

脚本会被延迟到整个页面都解析完毕后再运行。因此，在`<script>`元素上设置 defer 属性， 相当于告诉浏览器立即下载，但延迟执行。

HTML5 规范要求脚本应该按照它们出现的顺序执行，因此第一个推迟的脚本会在第二个推迟的脚本之前执行

不过在实际当中，推迟执行的脚本不一定总会按顺序执行，因此最好只包含一个这样的脚本。

---

async 异步执行脚本

给脚本添加 async 属性的目的是告诉浏览器，不必等脚本下载和执行完后再加载页面，同样也不必等到
该异步脚本下载和执行后再加载其他脚本

---

动态加载脚本

```js
const script = document.createElement('script');
script.src = 'gibberish.js';
document.head.appendChild(script);
```

在把 HTMLElement 元素添加到 DOM 且执行到这段代码之前不会发送请求

默认情况下，以这种方式创建的`<script>`元素是以异步方式加载的，相当于添加了 async 属性。

不过这样做可能会有问题，因为所有浏览器都支持 createElement()方法，但不是所有浏览器都支持 async 属性。

因此，如果要统一动态脚本的加载行为，可以明确将其设置为同步加载：

```js
const script = document.createElement('script');
script.src = 'gibberish.js';
script.async = false;
document.head.appendChild(script);
```

以这种方式获取的资源对浏览器预加载器是不可见的。这会严重影响它们在资源获取队列中的优先级。

根据应用程序的工作方式以及怎么使用，这种方式可能会严重影响性能。

要想让预加载器知道这些动态请求文件的存在，可以在文档头部显式声明它们：

```js
<link rel="preload" href="gibberish.js">
```
