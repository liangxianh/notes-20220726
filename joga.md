前段时间公司一个海外项目需要把链接分享到某App，某book等软件上，分享到聊天框的时候想要展示富文本（图片，标题，简介），如果在微信和qq上是有对应的api直接调取传参即可，但是外网的几个软件则需要Open Graph Protocol协议，也就是
<meta property="og:type" content="website">
<meta property="og:title" content="xxx">
<meta property="og:image" content="https://xxx.png"> 
<meta property="og:image:width" content="400">
<meta property="og:image:height" content="400">
<meta property="og:description" content="xxxxxxxx">
如果要展示的东西是静态的，那么如上即可，app分享的链接就是对应的页面，那么问题来了，动态展示的话这样操作是无效的，因为如今大部分项目都是前后端分离，og等meta标签是提供给爬虫去爬的，而爬虫有一个特点是不会执行你的js脚本，单靠前端赋值那么你的动态就会失效，最终爬出来的是空白，因此要解决这个问题就要从服务器方面入手，我用的方法是，分享的链接改为调取后台的接口，接口返回一个只含meta标签和一段跳转原分享页面的脚本内容，就是说展示给爬虫和最终打开看到的不是一个页面。
当然，要是你的项目是前后端不分离，或者使用ssr的，可以忽略了，因为你基本不会遇到这个问题
