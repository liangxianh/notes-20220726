### 一 html介绍

> 1 html 标签格式化元素

```
<b> font </b>
<strong> font </strong>
<em> font </em>
<i> font </i>
<mark> font </mark>
<small> font </small>
<del> font </del>
<ins> font </ins>
<sub> font </sub>
<sup> font </sup>
```


> 2 meta  viewport作用

<meta name="viewport" content="width=device-width,initial-scale=1">


> 3 meta name content添加作者和描述

许多 <meta> 元素包含了 name 和 content 属性：

name 指定了 meta 元素的类型；说明该元素包含了什么类型的信息。
content 指定了实际的元数据内容。
这两个 meta 元素对于定义你的页面的作者和提供页面的简要描述是很有用的。让我们看一个例子：
```
<meta name="author" content="Chris Mills">
<meta name="description" content="The MDN Web Docs Learning Area aims to provide
complete beginners to the Web with all they need to know to get
started with developing web sites and applications.">
```
指定包含关于页面内容的关键字的页面内容的描述是很有用的，因为它可能或让你的页面在搜索引擎的相关的搜索出现得更多（这些行为在术语上被称为：<b>搜索引擎优化，或 SEO</b>。）
<b>description 也被使用在搜索引擎显示的结果页中</b>

> 4 分享到facebook/twitter内容

比如MDN Web文档head中写如下内容，当你在 Facebook 上链接到 MDN 时，该链接将显示一个图像和描述：这为用户提供更丰富的体验。
Facebook 编写的元数据协议 [Open Graph Data](https://ogp.me/) 为网站提供了更丰富的元数据
```
<meta property="og:image" content="https://developer.mozilla.org/static/img/opengraph-logo.png">
<meta property="og:description" content="The Mozilla Developer Network (MDN) provides
information about Open Web technologies including HTML, CSS, and APIs for both Web sites
and HTML5 Apps. It also documents Mozilla products, like Firefox OS.">
<meta property="og:title" content="Mozilla Developer Network">
```
```
有很多内容，详见Facebook 编写的元数据协议 [Open Graph Data]
<head>
<title>The Rock (1996)</title>
<meta property="og:title" content="The Rock" />
<meta property="og:type" content="video.movie" />
<meta property="og:url" content="https://www.imdb.com/title/tt0117500/" />
<meta property="og:image" content="https://ia.media-imdb.com/images/rock.jpg" />
<meta property="og:audio" content="https://example.com/bond/theme.mp3" />
<meta property="og:description" 
  content="Sean Connery found fame and fortune as the
           suave, sophisticated British agent, James Bond." />
<meta property="og:determiner" content="the" />
<meta property="og:locale" content="en_GB" />
<meta property="og:locale:alternate" content="fr_FR" />
<meta property="og:locale:alternate" content="es_ES" />
<meta property="og:site_name" content="IMDb" />
<meta property="og:video" content="https://example.com/bond/trailer.swf" />
...
</head>
```
Twitter 还拥有自己的类型的专有元数据协议([称为：Twitter cards](https://developer.twitter.com/en/docs/tweets/optimize-with-cards/overview/abouts-cards)),当网站的 URL 显示在 twitter.com 上时，它具有相似的效果。例如下面：
```
<meta name="twitter:title" content="Mozilla Developer Network">
```

> 5 站点增加自定义图标

页面添加图标的方式有：

* 将其保存在与网站的索引页面相同的目录中，以 .ico 格式保存（大多数浏览器将支持更通用的格式，如 .gif 或 .png，但使用 ICO 格式将确保它能在如 Internet Explorer 6 那样古老的浏览器显示）

* 将以下行添加到 HTML 的 <head> 中以引用它：
  ```
  <link rel="icon" href="favicon.ico" type="image/x-icon">
  ```

 > 6 为文档设定主语言
  ```
  <html lang="zh-CN">
  ```
这在很多方面都很有用。如果你的 HTML 文档的语言设置好了，那么你的 HTML 文档就会被搜索引擎更有效地索引（例如，允许它在特定于语言的结果中正确显示），对于那些使用屏幕阅读器的视障人士也很有用（例如，法语和英语中都有“six”这个单词，但是发音却完全不同）。
    
[参考文档mdn](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Introduction_to_HTML/The_head_metadata_in_HTML)
    
    
> 7 html文字处理基础 	学习如何用标记 (段落、标题、列表、强调、引用) 来建立基础文本页面的文本结构和文本内容。(它的语义值将以多种方式被使用，比如通过搜索引擎和屏幕阅读器)
  
    * 六个标题元素标签 —— <h1>、<h2>、<h3>、<h4>、<h5>、<h6>
    
    1. 您应该最好只对每个页面使用一次<h1> — 这是顶级标题，所有其他标题位于层次结构中的下方。
    2. 请确保在层次结构中以正确的顺序使用标题。不要使用<h3>来表示副标题，后面跟<h2>来表示副副标题 - 这是没有意义的，会导致奇怪的结果。
    3. 在可用的六个标题级别中，您应该只在每页使用不超过三个，除非您认为有必要使用更多。具有许多级别的文档（即，较深的标题层次结构）变得难以操作并且难以导航。在这种情况下，如果可能，建议将内容分散在多个页面上。
    
    * 段落用<p>
    
    > 为什么需要结构化
    
    1. 用户在阅读网页时，往往会快速浏览以查找相关内容，经常只是阅读开头的标题（我们通常在一个网页上会花费很少的时间 spend a very short time on a web page)。如果用户不能在几秒内看到一些有用的内容，他们很可能会感到沮丧并离开。
    2. 对您的网页建立索引的搜索引擎将标题的内容视为影响网页搜索排名的重要关键字。没有标题，您的网页在<b>SEO（搜索引擎优化）</b>方面效果不佳。
    3. 严重视力障碍者通常不会阅读网页；他们用听力来代替。完成这项工作的软件叫做屏幕阅读器（screen reader）。该软件提供了快速访问给定文本内容的方法。在使用的各种技术中，它们通过朗读标题来提供文档的概述，让用户能快速找到他们需要的信息。如果标题不可用，用户将被迫听到整个文档的大声朗读。
    4. 使用CSS样式化内容，或者使用JavaScript做一些有趣的事情，你需要包含相关内容的元素，所以 CSS / JavaScript 可以有效地定位它。
    
    * 无序列表（ul, li）和有序列表(ol,li)
    
   >  重点强调<em> 非常重要<strong>
    
    这样做既可以让文档更加地有用，也可以被屏幕阅读器识别出来，并以不同的语调发出。而下面
    <b>, <i>, 和 <u> 粗体、斜体、下划线仅仅影响表象（不会对无障碍和seo有用）而且没有语义，被称为**表象元素（presentational elements）**并且不应该再被使用
      HTML5 用新的语义规则重新定义了 <b>、<i> 和 <u>,使得它们的语言显得稍微有点混乱。
    语义对无障碍，SEO（搜索引擎优化）等非常重要
     
      注意：使用下划线的忠告：因为我们常常会认为网页中的下划线代表着一个超链接**，**所以最好只用下划线来代表超链接。而在语义适合的情况下不得不使用<u>元素时，可以使用 CSS 来改变<u>元素对应的下划线的默认样式，从而和超链接的下划线区分开来。下面是一个具体的例子：
      
      ```
      <p>
        总有一天我会改掉写<u style="text-decoration-line: underline; text-decoration-style: wavy;">措字</u>的毛病。
      </p>
      ```
> 8 创建超链接 
      
      ```
       <a href="https://www.mozilla.org/zh-CN/" title="可以将甚至是块级内容放到a内，转换为链接">
        <img src="mozilla-image.png" alt="链接至 Mozilla 主页的 Mozilla 标志">
       </a>
       ```
      
    * 可以设置title包含关于链接的补充信息，例如页面包含什么样的信息或需要注意的事情。鼠标悬浮回显示出来；
    * 可以自定义创建自己的示例链接，被a标签包含的内容
    * 可以链接到项目的其他页面直接星队路径即可
      
      ```
      <p>点击打开<a href="../pdfs/project-brief.pdf">项目简介</a>。</p>
      ```
      
> 文档片段
      
      超链接除了可以链接到文档外，也可以链接到 HTML 文档的特定部分（被称为文档片段）。要做到这一点，你必须首先给要链接到的元素分配一个 id 属性。例如，如果你想链接到一个特定的标题，可以这样做：
      
      ```
      <h2 id="Mailing_address">邮寄地址</h2>
      ```
      
      同一个文档下的处理：
      <p>本页面底部可以找到<a href="#Mailing_address">公司邮寄地址</a>。</p>
      
      链接到某个特定页面（不同文档）的特定片段
      <p>要提供意见和建议，请将信件邮寄至<a href="contacts.html#Mailing_address">我们的地址</a>。</p>
      
> 链接最佳实践：
      
      ```
      <p><a href="https://www.mozilla.org/firefox/">
        下载 Firefox
       </a></p>
      ```
      
> 链接到非 HTML 资源——留下清晰的指示
      
      ```
      <p><a href="https://www.example.com/large-report.pdf">
        下载销售报告（PDF, 10MB）
       </a></p>

      <p><a href="https://www.example.com/video-stream/" target="_blank">
       观看视频（将在新标签页中播放，HD 画质）
      </a></p>

      <p><a href="https://www.example.com/car-game">
       进入汽车游戏（需要 Flash 插件）
      </a></p>
      ```
> 在下载链接时使用 download 属性
      
      当你链接到要下载的资源而不是在浏览器中打开时，你可以使用 download 属性来提供一个默认的保存文件名。下面是一个 Firefox 的 Windows 最新版本下载链接的示例：
      
      ```
      <a href="https://download.mozilla.org/?product=firefox-latest-ssl&os=win64&lang=zh-CN"
       download="firefox-latest-64bit-installer.exe">
     下载最新的 Firefox 中文版 - Windows（64 位）
      </a>
      ```
      
> 电子邮件链接
      
      ```
      <a href="mailto:nowhere@mozilla.org">向 nowhere 发邮件</a>
      也可以指定详细信息：任何标准的邮件头字段可以被添加到你提供的 mailto URL 中。其中最常用的是主题（subject）、抄送（cc）和主体（body）（这不是一个真正的标头字段，但允许你为新邮件指定一个简短的内容消息）。每个字段及其值被指定为查询项。
      <a href="mailto:nowhere@mozilla.org?      cc=name2@rapidtables.com&bcc=name3@rapidtables.com&subject=The%20subject%20of%20the%20email&body=The%20body%20of%20the%20email">
       Send mail with cc, bcc, subject and body
      </a>
      ```
      每个字段的值必须是使用 URL 编码的，即使用百分号转义的非打印字符（不可见字符比如制表符、换行符、分页符）和空格。同时注意使用问号（?）来分隔主 URL 与参数值，以及使用 & 符来分隔 mailto: URL 中的各个参数。这是标准的 URL 查询标记方法。阅读 GET 方法以了解哪种 URL 查询标记方法是更常用的。
      
      
      
> 10 高级文本格式
      
    > 描述列表（dl(description list) dt(description ) dd(description definition)）
      
      ```
      <dl>
       <dt>内心独白</dt>
         <dd>戏剧中，某个角色对自己的内心活动或感受进行念白表演，这些台词只面向观众，而其他角色不会听到。</dd>
       <dt>语言独白</dt>
         <dd>戏剧中，某个角色把自己的想法直接进行念白表演，观众和其他角色都可以听到。</dd>
     </dl>
      ```
      
      浏览器的默认样式会在描述列表的描述部分（description definition）和描述术语（description terms）之间产生缩进(dd元素回存在一个margin-inline-start样式，可以通过这个改变缩进的长度；)
      
    > 引用
      
      HTML 也有用于标记引用的特性，至于使用哪个元素标记，取决于你引用的是一块还是一行。
      块引用<blockquote>
      行内引用<q>
      引文cite 可以作为属性页可以作为标签：cite属性内容不会被浏览器显示、屏幕阅读器阅读，需使用 JavaScript 或 CSS，浏览器才会显示cite的内容。如果你想要确保引用的来源在页面上是可显示的，更好的方法是为<cite>元素附上链接：
      
      ```
      <blockquote cite="https://developer.mozilla.org/en-US/docs/Web/HTML/Element/blockquote">
        <p>The <strong>HTML <code>&lt;blockquote&gt;</code> Element</strong> (or <em>HTML Block
         Quotation Element</em>) indicates that the enclosed text is an extended quotation.</p>
      </blockquote>

      <p>The quote element — <code>&lt;q&gt;</code> — is <q cite="https://developer.mozilla.org/en-US/docs/Web/HTML/Element/q">intended
        for short quotations that don't require paragraph breaks.</q> -- <a href="https://developer.mozilla.org/en-US/docs/Web/HTML/Element/q">
        <cite>MDN q page</cite></a>.
      </p>
      ```
      
   > 缩略语 
      
     另一个你在 web 上看到的相当常见的元素是<abbr>——它常被用来包裹一个缩略语或缩写，并且提供缩写的解释（包含在title属性中）。让我们看看下面两个例子：
      ```
      <p><abbr title="美国国家航空航天局（National Aeronautics and Space Administration）">NASA</abbr> 做了一些动人心弦的事情。</p>
      ```
      这些代码的显示效果如下（当光标移动到项目上时会出现提示title内容）：
      
   > 标记联系方式
      
     HTML 有个用于标记联系方式的元素——<address>。它仅仅包含你的联系方式，例如：
      ```
      <address>
        <p>Chris Mills, Manchester, The Grim North, UK</p>
      </address>
      ```
      
   > 上标和下标<sup> 和<sub>
      
   > 展示计算机代码
      
     * <code>: 用于标记计算机通用代码。
     * <pre>: 用于保留空白字符（通常用于代码块）——如果您在文本中使用缩进或多余的空白，浏览器将忽略它，您将不会在呈现的页面上看到它。但是，如果您将文本包含在<pre></pre>标签中，那么空白将会以与你在文本编辑器中看到的相同的方式渲染出来。
     * <var>: 用于标记具体变量名。
     * <kbd>: 用于标记输入电脑的键盘（或其他类型）输入。
     * <samp>: 用于标记计算机程序的输出。
    
  > 标记时间和日期
  但是这些不同的格式不容易被电脑识别 — 假如你想自动抓取页面上所有事件的日期并将它们插入到日历中，<time> 元素允许你附上清晰的、可被机器识别的 时间/日期来实现这种需求。
  ```
  <time datetime="2016-01-20">2016 年 1 月 20 日</time>
  ```
> 11 文档和网站架构

  * 文档的基本组成部分
  
    
    1. 页眉  <header>
    2. 导航栏  <nav>
    3. 主内容  <main>内部可以有子内容区段 <article> <section> <div>
    4. 侧边栏 <aside> 经常嵌套在main中
    5. 页脚 <footer>
    
     * <main> 存放每个页面独有的内容。每个页面上只能用一次 <main>，且直接位于 <body> 中。最好不要把它嵌套进其它元素。
     * <article> 包围的内容即一篇文章，与页面其它部分无关（比如一篇博文）。
     * <section> 与 <article> 类似，但 <section> 更适用于组织页面使其按功能（比如迷你地图、一组文章标题和摘要）分块。一般的最佳用法是：以 标题 作为开头；也可以把一篇 <article> 分成若干部分并分别置于不同的 <section> 中，也可以把一个区段 <section> 分成若干部分并分别置于不同的 <article> 中，取决于上下文。
     * <aside> 包含一些间接信息（术语条目、作者简介、相关链接，等等）。
     * <header> 是简介形式的内容。如果它是 <body> 的子元素，那么就是网站的全局页眉。如果它是 <article> 或<section> 的子元素，那么它是这些部分特有的页眉（此 <header> 非彼 标题）。
     * <nav> 包含页面主导航功能。其中不应包含二级链接等内容。
     * <footer> 包含了页面的页脚部分。
   
  * 无语义元素 div span
 
  * 换行与水平分割线<br> <hr>
 
     <br> 可在段落中进行换行
     <hr> 元素在文档中生成一条水平分割线，表示文本中主题的变化（例如话题或场景的改变）。一般就是一条水平的直线
 
 
 > 12 html调试 
 
 最好的方法就是让你的 HTML 页面通过 [Markup Validation Service](https://validator.w3.org/)。由 W3C（制定 HTML、CSS 和其他网络技术标准的组织）创立并维护的标记验证服务。把一个 HTML 文档加载至本网页并运行，网页会返回一个错误报告。网页可以接受网址、上传一个 HTML 文档，或者直接输入一些 HTML 代码。
 

### 二 多媒体与嵌入

> 1 图片 

 * <img> src alt(备选文案) title属性 width height（单位无（默认px），px，% 都可以）
 
 可以通过搭配文字来解说图片如下两种方式：
 ```
 <div class="figure">
  <img src="/images/dinosaur_small.jpg"
     alt="一只恐龙头部和躯干的骨架，它有一个巨大的头，长着锋利的牙齿。"
     width="400"
     height="341">
  <p>曼彻斯特大学博物馆展出的一只霸王龙的化石</p>
</div>

有一个更好的做法是使用 HTML5 的 <figure> 和 <figcaption> 元素，它正是为此而被创造出来的：为图片提供一个语义容器，在标题和图片之间建立清晰的关联
<figure>
  <img src="https://raw.githubusercontent.com/mdn/learning-area/master/html/multimedia-and-embedding/images-in-html/dinosaur_small.jpg"
      alt="一只恐龙头部和躯干的骨架，它有一个巨大的头，长着锋利的牙齿。"
      width="400"
      height="341">
  <figcaption>曼彻斯特大学博物馆展出的一只霸王龙的化石</figcaption>
</figure>
 ```
 
 
 * css背景图
 
 CSS 背景图片只是为了装饰 — 如果你只是想要在你的页面上添加一些漂亮的东西，来提升视觉效果，那 CSS 的做法是可以的。但是这样插入的图片完全没有语义上的意义，它们不能有任何备选文本，也不能被屏幕阅读器识别。这就是 HTML 图片有用的地方了。
 ```
 
 ```
 
 <div style="color: red;">
警告： 大多数图片是有版权的。不要在你的网页上使用一张图片，除非：
    你是图片版权所有者
    你有图片版权所有者明确的、书面上的使用授权
    你有充分的证据证明这张图片是公共领域内的
    侵犯版权是违法并且不道德的。此外，在得到授权之前永远不要把你的src属性指向其他人网站上的图片。这被称为"盗链（hotlinking）"。同样，盗取其他人的带宽也是违法的。而且这会降低你的页面的加载速度，而且图片可能会在不受你控制的情况下被移走或用别的令人尴尬的东西替换掉。
  </div>
    
    
 > 2 视频和音频 <video> 和 <audio> 标签
 
 ```
 <video src="rabbit320.webm" controls>
  <p>你的浏览器不支持 HTML5 视频。可点击<a href="rabbit320.mp4">此链接</a>观看</p>
 </video>
 
 <video> 标签内的内容
这个叫做后备内容 — 当浏览器不支持 <video> 标签的时候，就会显示这段内容，这使得我们能够对旧的浏览器提供回退内容
使用多个播放源以提高兼容性

如果你尝试使用像 Safari 或者 Internet Explorer 这些浏览器来访问上面的链接。视频并不会播放，这是因为不同的浏览器对视频格式的支持不同

<video controls>
  <source src="rabbit320.mp4" type="video/mp4">
  <source src="rabbit320.webm" type="video/webm">
  <p>你的浏览器不支持 HTML5 视频。可点击<a href="rabbit320.mp4">此链接</a>观看</p>
</video>
浏览器将会检查 <source> 标签，并且播放第一个与其自身 codec 相匹配的媒体
 ```
* width 和 height

你可以用属性控制视频的尺寸，也可以用 CSS 来控制视频尺寸。无论使用哪种方式，视频都会保持它原始的长宽比 — 也叫做纵横比。如果你设置的尺寸没有保持视频原始长宽比，那么视频边框将会拉伸，而未被视频内容填充的部分，将会显示默认的背景颜色。

* autoplay
这个属性会使音频和视频内容立即播放，即使页面的其他部分还没有加载完全。建议不要应用这个属性在你的网站上，因为用户们会比较反感自动播放的媒体文件。

* loop
这个属性可以让音频或者视频文件循环播放。同样不建议使用，除非有必要。

* muted
这个属性会导致媒体播放时，默认关闭声音。

* poster
这个属性指向了一个图像的 URL，这个图像会在视频播放前显示。通常用于粗略的预览或者广告。

* preload
这个属性被用来缓冲较大的文件，有 3 个值可选：
```
    "none" ：不缓冲
    "auto" ：页面加载后缓存媒体文件
    "metadata" ：仅缓冲文件的元数据
```

audio 不支持width/height以及poster属性

> 你都可以在 Javascript 中调用 load() 方法来重置媒体。如果有多个由 <source> 标签指定的媒体来源，浏览器会从选择媒体来源开始重新加载媒体。
```
const mediaElem = document.getElementById("my-media-element");
mediaElem.load();
```
    
> 音轨增删事件
```
const mediaElem = document.querySelector("video");
mediaElem.audioTracks.onaddtrack = function(event) {
  audioTrackAdded(event.track);
}
```

> 显示音轨文本
文本轨道会使你的网站更容易被搜索引擎抓取到（SEO），由于搜索引擎的文本抓取能力非常强大，使用文本轨道甚至可以让搜索引擎通过视频的内容直接链接。
以 .vtt 后缀名保存文件。
用 <track> 标签链接 .vtt 文件， <track> 标签需放在 <audio> 或 <video> 标签当中，同时需要放在所有 <source> 标签之后。使用 kind 属性来指明是哪一种类型，如 subtitles、captions、descriptions。然后，使用 srclang 来告诉浏览器你是用什么语言来编写的 subtitles。

 ```
 <video controls>
    <source src="example.mp4" type="video/mp4">
    <source src="example.webm" type="video/webm">
    <track kind="subtitles" src="subtitles_en.vtt" srclang="en">
</video>

 ```
    
    
 3 从对象到iframe---其他嵌入技术 iframe object svg canvas 等
    
在这篇文章中，我们将直接进入自主学习部分，让你立即体会到嵌入技术的实用性。大家都非常熟悉Youtube，但很多人不了解它所提供的一些分享功能。让我们来看看 Youtube 如何让我们通过<iframe>在页面中嵌入喜欢的视频。

首先，去 Youtube 找一个喜欢的视频。
在视频下方，你会看到一个共享按钮 - 点击查看共享选项。
选择“ *嵌入”*选项卡，你将得到一些 <iframe> 代码——复制一下。
粘贴到下面的输入框里，看看输出结果是什么。
此外，你还可以试试在示例中嵌入 Google 地图：

去 Google 地图找一个喜欢的地图。
点击 UI 左上角的“汉堡菜单”（三条水平线）。
选择共享或嵌入地图选项。
选择嵌入地图选项，这将给你一些<iframe>代码 - 复制一下。
粘贴到下面的输入框，看看输出结果是什么。

利用object可以把pdf嵌入到页面上
    
```
  <p>点击打开<a href="../pdfs/Statement.pdf">项目简介</a></p>

  <object data="../pdfs/Statement.pdf" type="application/pdf" width="200" height="300" typemustmatch>
    <p>You don't have a PDF plugin, but you can <a href="myfile.pdf">download the PDF file.</a></p>
  </object>
  <!--  注意object嵌入pdf在移动端屏幕会显示不出来  -->
```
    
    
    
