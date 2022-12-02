1 calc  calculate 的缩写，CSS3 新增，为元素指定动态宽度、长度等，注意此处的动态是计算之后得一个值

```
.box {
  padding: 10px;
  width: calc(90% - 30px);
  background-color: rebeccapurple;
  color: white;
}
```

2 @规则

```
@import 'styles2.css';

@media (min-width: 30em) {
  body {
    background-color: blue;
  }
}


@keyframes rocketAni {
    0% {
        transform: translate(0, 0)
    }

    33% {
        transform: translate(2px, -5px);
    }

    66% {
        transform: translate(-2px, 0);
    }

    100% {
        transform: translate(2px, 5px);
    }
}
.rocketAniC{
    -webkit-animation: rocketAni 0.2s linear infinite alternate;
    -moz-animation: rocketAni 0.2s linear infinite alternate;
    -o-animation: rocketAni 0.2s linear infinite alternate;
    animation: rocketAni 0.2s linear infinite alternate;
}
```

3 css 何时加载如果工作
![image](https://user-images.githubusercontent.com/31762176/204702101-cb1a94a2-3394-49fa-80e0-2d198af92952.png)

* 浏览器载入 HTML 文件（比如从网络上获取）
* 将 HTML 文件转化成一个 DOM（Document Object Model），DOM 是文件在计算机内存中的表现形式，下一节将更加详细的解释 DOM。
* 接下来，浏览器会拉取该 HTML 相关的大部分资源，比如嵌入到页面的图片、视频和 CSS 样式。JavaScript 则会稍后进行处理，简单起见，同时此节主讲 CSS，所以这里对如何加载 JavaScript 不会展开叙述。
* 浏览器拉取到 CSS 之后会进行解析，根据选择器的不同类型（比如 element、class、id 等等）把他们分到不同的“桶”中。浏览器基于它找到的不同的选择器，将不同的规则（基于选择器的规则，如元素选择器、类选择器、id 选择器等）应用在对应的 DOM 的节点中，并添加节点依赖的样式（这个中间步骤称为渲染树）。
* 上述的规则应用于渲染树之后，渲染树会依照应该出现的结构进行布局。
网页展示在屏幕上（这一步被称为着色）。

```

```

4 盒模型

[盒模型](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/The_box_model#%E8%A1%A5%E5%85%85%EF%BC%9A%E5%86%85%E9%83%A8%E5%92%8C%E5%A4%96%E9%83%A8%E6%98%BE%E7%A4%BA%E7%B1%BB%E5%9E%8B)
[外边距会重叠](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Box_Model/Mastering_margin_collapsing)
