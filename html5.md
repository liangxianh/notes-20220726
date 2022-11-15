1 html 标签格式化元素

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


2 meta  viewport作用
<meta name="viewport" content="width=device-width,initial-scale=1">


3 meta name content添加作者和描述
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

4 分享到facebook/twitter内容，比如MDN Web文档head中写如下内容，当你在 Facebook 上链接到 MDN 时，该链接将显示一个图像和描述：这为用户提供更丰富的体验。
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
> 8 
    
> 9 
    
> 10 
    
> 11 
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
