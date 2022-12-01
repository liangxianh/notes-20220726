### 基于vue vant 的部分商城类的项目

一 项目构建从无到有
```
npm install -g vue-cli(没反应，可以修改一下镜像)(vue-cli 新版本已经更新了名字@vue/cli)
npm config set registry http://registry.npm.taobao.org/
npm install -g @vue/cli

安装其他包，注意
-D  --save-dev。包将保存在devDependencies
-S  --save 包将保存在dependencies

注意vue和vue-router/vuex版本需要对应使用
使用vue2安装对应的router3版本;
使用vue3安装对应的router4版本;

vue2 对应vuex3, vue3 对应vuex4
```

二 遇到的问题总结

1. vue-router 使用报错 Uncaught TypeError: Cannot read properties of undefined (reading 'install')

```
bootstrap:27 Uncaught TypeError: Cannot read properties of undefined (reading 'install')
    at Vue.use (vue.runtime.esm.js:5739:31)
    at ./src/router/index.js (index.js:8:1)
    at __webpack_require__ (bootstrap:24:1)
    at fn (hot module replacement:62:1)
    at ./src/main.js (App.vue:7:1)
    at __webpack_require__ (bootstrap:24:1)
    at startup:7:92
    at __webpack_require__.O (chunk loaded:25:1)
    at startup:8:1
    at startup:8:1
```
解决方案
```
使用vue2安装对应的router3版本;
使用vue3安装对应的router4版本;

vue-router 默认安装最新版本，目前是4.x

这里使用的是vue2,所以要使用 vue-router 3.x 版本

npm i vue-router@3
而且需要注意vue-router的引入方式不同
vue-router4--- 文件引入后在main.js里需要Vue.use(router)
vue-router3---直接在new Vue下面挂载即可
```

2. 

```
ValidationError: Invalid configuration object. Webpack has been initialized using a configuration object that does not match the API schema.
         - configuration has an unknown property 'css'. These properties are valid:
         **
         **
> Options object as provided by the user.
           For typos: please correct them.
           For loader options: webpack >= v2.0.0 no longer allows custom properties in configuration.
             Loaders should be updated to allow passing options via loader options in module.rules.
             Until loaders are updated one can use the LoaderOptionsPlugin to pass these options to the loader:
npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! app-h5@0.1.0 serve: `vue-cli-service serve`
npm ERR! Exit status 1
npm ERR! 
npm ERR! Failed at the app-h5@0.1.0 serve script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

版本：
webpack: 5.74.0
webpack-cli: 4.10.0


Failed to compile with 1 error                               上午9:28:10

 error  in ./src/App.vue?vue&type=style&index=0&id=7ba5bd90&lang=css&

Syntax Error: ValidationError: Invalid options object. PostCSS Loader has been initialized using an options object that does not match the API schema.
 - options has an unknown property 'plugins'. These properties are valid:
   object { postcssOptions?, execute?, sourceMap?, implementation? }

```
注意配置的层级，configurewebpack css以及devserver是与其并列的；

postcss-px2rem需要依赖postcss-loader


3. serve时报node-module的错误 ，如何关闭

```
ERROR  Failed to compile with 13 errors 
 error  in ./node_modules/@vant/use/dist/index.esm.mjs
export 'isVNode' (imported as 'isVNode') was not found in 'vue' (possible export
```




[eslint关闭各种内容的操作]（https://segmentfault.com/a/1190000040036418）


4 使用vue-i18n时注意有可能因为版本不匹配而报错
```
i18n报错， Cannot read properties of undefined (reading ‘install‘) at Function.Vue.use

"vue": "^2.6.14",
    "vue-i18n": "^9.2.2",
```
报错信息：i18n报错， Cannot read properties of undefined (reading ‘install‘) at Function.Vue.use。

报错的原因主要是因为当前使用的版本不匹配，在这里提供一个可行的其中一种方法。
下载@8版本的就好
npm install vue-i18n@8


5 vant 国际化后但是 sku无格式化如何做国际化处理，没有找到具体的方案自己查看了源码，总结的相关方案如下:
```
import Vue from 'vue';
import { Locale } from 'vant';
import enUS from 'vant/es/locale/lang/en-US';
// 2. 引入组件样式
import 'vant/lib/index.css';

Locale.use('en-US', enUS);

import {
  Field,
  Radio,
} from 'vant';

Vue.use(Field)
Vue.use(RadioGroup);

// sku 组件单独做国际化处理
const messages = {
  'en-US': {
    vanSku: {
      select: 'Please select',
      selected: 'Selected',
      selectSku: 'Please select product specifications first',
      soldout: 'Out of stock',
      originPrice: 'Original Price',
      minusTip: 'Select at least one',
      minusStartTip: function minusStartTip(start) {
        return start + "\u4EF6\u8D77\u552E";
      },
      unavailable: 'The product is no longer available for purchase!',
      stock: 'Remains',
      stockUnit: 'items',
      quotaTip: function quotaTip(quota) {
        // if (quota < 2) {
        //   return "Limit to " + quota + "item per person";
        // }
        return "Limit to " + quota + quota<2?" item":" items" + " per person";
      },
      quotaUsedTip: function quotaUsedTip(quota, count) {
        // let str1 = ''
        // let str2 = ''
        // if (count < 2) {
        //   str1 = "Limit to " + quota + "item per person";
        // } else {
        //   str1 = "Limit to " + quota + "items per person";
        // }
        return "Limit to " + quota + quota<2?" item":" items" + " per person" + ",you have already purchased" + count + count<2?" item":" items";
      }
    },
    vanSkuActions: {
      buy: 'Buy Now',
      addCart: 'Add to Cart'
    },
    vanSkuStepper: {
      quotaLimit: function quotaLimit(quota) {
        // return "Limited to" + quota + " items";
        return ''
      },
      quotaStart: function quotaStart(start) {
        return "Starting from " + start + "items";
      },
      comma: ',',
      num: 'Number of purchases'
    }
  }
};
Locale.add(messages);
```
messages里面的信息是在sku---node_modules/vant/es/sku/lang.js 可以找到具体的语言用变量名，直接进行替换即可；


6 vant sku 组件 里的price默认会除100？
可以利用插槽来解决
```
 <van-sku
      v-model="show"
      :sku="sku"
      :goods="goods"
      :goods-id="productId"
      :hide-stock="sku.hide_stock"
      :quota="quota"
      :initial-sku="initialSku"
      reset-stepper-on-hide
      reset-selected-sku-on-hide
      disable-stepper-input
      @buy-clicked="toBuy"
      :show-add-cart-btn="false"
      buy-text="Buy Now"
    >
      <template #sku-header-price="props">
        <div class="van-sku__goods-price">
          <span class="van-sku__price-symbol"
            >{{ detailObj.currencySymbol }}
          </span>
          <!-- //重点注意 此处应设置vant手册中的初始化sku选择列表 不然只能支持单属性价格问题 -->
          <span class="van-sku__price-num">{{
            props.selectedSkuComb.price
          }}</span>
        </div>
      </template>
    </van-sku>
```

7 vant 主题定制
注意包版本问题
"less": "^4.1.3",
"less-loader": "^7.3.0",
```
const myTheme = path.join(__dirname, './src/assets/styles/var.less')


  css: {
    loaderOptions: {
      less: {
        javascriptEnabled: true,
	        modifyVars: {
	          // 直接覆盖变量
	          // 'text-color': 'red',
	          // 或者可以通过 less 文件覆盖（文件路径为绝对路径）
            'hack': `true; @import "${myTheme}";`
	        }
      },
    },
  },

当类似这么引入时会报错如下 
ERROR in ./src/views/Agent.vue?vue&type=style&index=0&id=23dc028a&lang=less&scoped=true& (./node_modules/css-loader/dist/cjs.js??clonedRuleSet-32.use[1]!./node_modules/@vue/cli-service/node_modules/@vue/vue-loader-v15/lib/loaders/stylePostLoader.js!./node_modules/@vue/cli-service/node_modules/postcss-loader/dist/cjs.js??clonedRuleSet-32.use[2]!./node_modules/less-loader/dist/cjs.js??clonedRuleSet-32.use[3]!./node_modules/@vue/cli-service/lib/config/vue-loader-v15-resolve-compat/vue-loader.js??vue-loader-options!./src/views/Agent.vue?vue&type=style&index=0&id=23dc028a&lang=less&scoped=true&)

Invalid options object. Less Loader has been initialized using an options object that does not match the API schema.
 - options has an unknown property 'modifyVars'. These properties are valid:
   object { lessOptions?, additionalData?, sourceMap?, webpackImporter?, implementation? }
   
需要将less降级为5.*版本，即可
```
在文件/node_modules/vant/lib/style/mixins/var.less 找到对应的变量内容,更改要变换的内容；然后引入
```
以改变nav头部背景及文字颜色为例：可以通过改变样式来处理，
.van-nav-bar {
    background-color: @color-primary;

    .van-nav-bar__left {
      .van-icon-arrow-left {
        color: #fff;
        color: @font-white;
      }
    }
    .van-nav-bar__right {
      .van-nav-bar__text {
        color: @font-white;
      }
    }
    .van-nav-bar__title {
      color: @font-white;
    }
  }
  
也可以通过改变主题色方式来处理在src/assets/styles/var.less中定义如下：
@color-primary: #10AD94;
@font-white: rgba(256, 256, 256, 0.9);


@nav-bar-text-color: @font-white;
@nav-bar-title-text-color: @font-white;
@nav-bar-icon-color: @font-white;
@nav-bar-background-color: @color-primary;
都可以达到效果
```

8 better-scroll 设置时一定注意需要定位,且层级关系固定
详见[该文章](https://blog.csdn.net/qiqi_77_/article/details/79361413)

9 移动端底部有固定定位的按钮，body有输入文本框，focus时 按钮会将内容覆盖
原理即：监控window.innerHeight变化来改变定位方式，利用resize以及watch实现；
[详细解决方案参考](https://blog.csdn.net/Tmengxing/article/details/122685087)
[vue.config.js一些配置解释](https://blog.csdn.net/momomo12138/article/details/125844249)

10 利用terser-webpack-plugin去除console.log不起作用？？？？？？？？？？？？
```
具体代码如下：
  configureWebpack: {
    optimization: {
      minimize: true,
      minimizer: [
        new TerserWebpackPlugin({
          terserOptions: {
            compress: {
              warnings: false,
              drop_console: true,
              drop_debugger: true,
              pure_funcs: ['console.log', "console.table"] // 删除console
            }
          },
        }),
      ]
    }
   }
    
    
```
build后的文件仍然含有console.log，求救？？？？？？？
[插件api地址](https://webpack.js.org/plugins/terser-webpack-plugin/#terseroptions)
使用如下方式即可
```
 configureWebpack: (config) => {
    config.optimization.minimizer[0].options.minimizer.options.compress.drop_console = true
  },
```
具体详见webpack笔记

11 标签使用v-html富文本，如何让富文本中的图片点击放大；
```
外层绑定函数，获取内部具体哪个标签被点击了，若是img 则获取相应的url在弹框中打开并放大；
    showImg(e) {
      if (e.target.tagName == 'IMG') {
        this.previewImg = e.target.src
        this.imgShow = true
      }
    },
```
类似功能原生实现方法，可以用于webview
```
  <div id="mydiv">
    此处是你的富文本
    My first product. ------ <br><img class="zoom-in"
      src="yourimgurl"
      style="width: 50%; cursor: pointer;" image="" width="50%"><br><br>.-------++-------<br>End ddd
  </div>
  <script>
    function zoomin(event) {
      if (event.target.tagName == "IMG") {
        console.log(event.target.src)
        // 调用你的方法给到自己
      }
    }
    var mydiv = document.getElementById('mydiv')
    mydiv.addEventListener('click', zoomin)

  </script>
```

[参考文档](https://www.jianshu.com/p/936b10460cad)

12 富文本客户端想要图片固定宽高（且想要高等于宽），摘取部分展示不进行压缩
可以使用vw设置宽和高，在利用object-fit：设置相应的属性 [详细内容介绍参考](https://developer.mozilla.org/zh-CN/docs/Web/CSS/object-fit)
object-fit CSS 属性指定可替换元素（例如：<img> 或 <video>）的内容应该如何适应到其使用高度和宽度确定的框。
```
  <div id="mydiv">
    <img src="https://raw.githubusercontent.com/wasabeef/art/master/chip.jpg"
      alt="https://raw.githubusercontent.com/wasabeef/art/master/chip.jpg"
      style="width: 44vw; height:44vw;object-fit: cover;">
    <img src="https://raw.githubusercontent.com/wasabeef/art/master/chip.jpg"
      alt="https://raw.githubusercontent.com/wasabeef/art/master/chip.jpg"
      style="width: 44vw; height:44vw;object-fit: cover;">
  </div>
```
也可以通过设置 aspect-ratio: 1以及width:50%;来设置宽高比例1
aspect-ratio宽高比，纵横比；aspect-ratio：宽/高
```
<div id="mydiv">
    <img src="https://test-ap-cdn-paytrigger.shalltry.com/amatooke/file/0/1667961244153_CMP_20221109103403160.jpg"
      alt="details" image="" style="object-fit: cover; aspect-ratio: 1;" width="50%">
    <img src="https://test-ap-cdn-paytrigger.shalltry.com/amatooke/file/0/1667961244153_CMP_20221109103403160.jpg"
      alt="details" image="" style="object-fit: cover; aspect-ratio: 1;" width="50%">
</div>
```
	
	
13 富文本传递图片设置的vw样式被解析为px 为何？具体显示如下图；
![image](https://user-images.githubusercontent.com/31762176/200728109-5a5d6e63-e2c7-4bd0-9368-b4411d738d98.png)

需要注意的是，当给图片直接设置width属性等 需要注意单位问题；
<b>在 HTML 4.01 中，宽度应该被定义为以像素为单位或者以包含元素的百分比为单位。在 HTML5 中，宽度值必须以像素为单位。</b>
```
<img src="https://test-ap-cdn-paytrigger.shalltry.com/amatooke/file/0/1667961244153_CMP_20221109103403160.jpg"
      alt="details" image="" style="object-fit: cover;" width="50vw" height="50vw">
会被浏览器识别为width和height均为50px
```	
14 富文本中img标签包含样式内容，vue项目中通过v-html插入后style消失？
是因为xss 将标签的style过滤了，class都被过滤了，若想设置样式可以自己在文件中写，或者研究一下如何过滤xss

```
const tag = xss.getDefaultWhiteList()

// tag.img = ['style'] 

const myxss0 = new xss.FilterXSS()
const testcontent0 = myxss0.process('<img src="https://test-ap-cdn-paytrigger.shalltry.com/amatooke/file/0/1667974618908_CMP_20221109141657571.jpg" alt="details" image="" style="width: 49vw;height:49vw;margin-left: 0.5vw;margin-right:0.5vw;object-fit: cover;">')
console.log(testcontent0, '-------------testcontent0')

const myxss1 = new xss.FilterXSS({tag})
const testcontent1 = myxss1.process('<img src="https://test-ap-cdn-paytrigger.shalltry.com/amatooke/file/0/1667974618908_CMP_20221109141657571.jpg" alt="details" image="" style="width: 49vw;height:49vw;margin-left: 0.5vw;margin-right:0.5vw;object-fit: cover;">')
console.log(testcontent1, '-------------testcontent1')

const myxss2 = new xss.FilterXSS({
  css: {
    whiteList: {
      style: true,
      width: true,
      background: false
    }
  }
},tag)
const testcontent2 = myxss2.process('<img src="https://test-ap-cdn-paytrigger.shalltry.com/amatooke/file/0/1667974618908_CMP_20221109141657571.jpg" alt="details" image="" style="width: 49vw;height:49vw;margin-left: 0.5vw;margin-right:0.5vw;object-fit: cover;">')
console.log(testcontent2, '-------------testcontent2')
	
```
[参考文章1：vue 解决v-html指令存在xss漏洞](https://juejin.cn/post/7089978256102801439)
[参考文章2：XSS绕过姿势](https://cloud.tencent.com/developer/article/1480572)
[参考文章3：XSS攻击绕过过滤方法大全（约100种）]()
[参考文章4：How to use the xss/dist/xss.filterXSS function in xss](https://snyk.io/advisor/npm-package/xss/functions/xss%2Fdist%2Fxss.filterXSS)
[参考文章5：XSS插入绕过一些方式总结](https://blog.csdn.net/qq_29277155/article/details/51320064)
	
15 如下二维数组 进行数量相乘，属性拼接 js的逻辑转化
```
数据格式一：
    let tree = [
      {
        k_s: 's1',
        v: [
          {
            id: '1',
            name: '红色',
          },
          {
            id: '2',
            name: '蓝色',
          }
        ],
      },
      {
        k_s: 's2',
        v: [
          {
            id: '3',
            name: 'm',
          },
          {
            id: '4',
            name: 'l',
          }
        ],
      },
      {
        k_s: 's3',
        v: [
          {
            id: '5',
            name: '#',
          },
          {
            id: '6',
            name: '@',
          },
          {
            id: '7',
            name: '¥',
          },
        ],
      }
    ]	
转换为数据格式二如下：
    let obj = [
      {
        s1: 1,
        s2: 3,
        specs: '红色/m'
      },
      {
        s1: 1,
        s2: 4,
        specs: '红色/l'
      },
      {
        s1: 2,
        s2: 3,
        specs: '蓝色/m'
      },
      {
        s1: 2,
        s2: 4,
        specs: '蓝色/l'
      }
    ]
	
```
利用递归或者reduce
```
    let result = tree.reduce((pre, next) => {
      if (pre.length === 0) {
        for (let i = 0; i < next.v.length; i++) {
          var k = { specs: next.v[i].name }
          k[next.k_s] = next.v[i].id
          pre.push(k);
        }
        return pre
      }

      const newPre = [];

      for (let i = 0; i < next.v.length; i++) {
        for (let j = 0; j < pre.length; j++) {
          let k = {
            ...pre[j],
	    // 注意specs要在...pre[j]下面，可以覆盖pre本身的specs属性（否则每次都会是替换）
            specs: `${pre[j].specs}/${next.v[i].name}`,
          }
          k[next.k_s] = next.v[i].id
          newPre.push(k)
        }
      }

      return newPre
    }, [])
    console.log(result)	
```
android 端实现利用递归
```
  public List<String> getString(List<List<String>> list, int index, List<String> resultList) {
        if (list.size() == index) {
            return resultList;
        } else {
            List<String> temp = new ArrayList<>();
            if (resultList.size() == 0) {
                temp = list.get(index);
            } else {
                List<String> list1 = list.get(index);
                for (String s : resultList) {
                    for (String s1 : list1) {
                        String title = s + s1;
                        temp.add(title);
                    }
                }
            }
            return getString(list, index + 1, temp);
        }
    }	
        List<String> strings = new ArrayList<>();
        strings.add("11");
        strings.add("22");
        List<String> strings1 = new ArrayList<>();
        strings1.add("33");
        strings1.add("44");

        List<List<String>> list = new ArrayList<>();
        list.add(strings);
        list.add(strings1);
        int size = list.size();
        List<String> string = getString(list, 0, new ArrayList<>());
```

16 实现复制到剪切板

```
if (navigator.clipboard) {
        // clipboard api 复制
        navigator.clipboard.writeText(this.shareLink);
      } else {
        var textarea = document.createElement('textarea');
        document.body.appendChild(textarea);
        // 隐藏此输入框
        textarea.style.position = 'fixed';
        textarea.style.clip = 'rect(0 0 0 0)'; //deprecated
        textarea.style.top = '10px';
        // 赋值
        textarea.value = this.shareLink;
        // 选中
        textarea.select();
        // 复制
        document.execCommand('copy', true);   // deprecated
        // 移除输入框
        document.body.removeChild(textarea);
      }
      // this.$toast('复制成功可以去粘贴分享了')
      this.showShareTip = true
    },
```
问题1: navigator.clipboard对象 undefined：
答：undefined原因是因为navigator.clipboard对象只能在安全网络环境中才能使用，也就是，localhost中，或者https中才能正常使用，直接用ip地址访问时不可以的呢
	
	
问题2: 上述方法在浏览器中可以正常运行，但是在webview中失效
答：可以在android添加方法(需要客户端帮忙，说是添加无效)
	[参考文档1](https://github.com/microsoft/vscode/pull/116597)
	[参考文档2](https://www.lmlphp.com/user/151035/article/item/4336111/)

	
实现方式二，[参考](https://blog.csdn.net/qq_58340302/article/details/124480086)
	![image](https://user-images.githubusercontent.com/31762176/204955106-34881a5c-1545-4d60-963c-6bd6aafe3eb4.png)

	
实现方式三 [参考](https://blog.csdn.net/qq_58340302/article/details/124745813?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22124745813%22%2C%22source%22%3A%22qq_58340302%22%7D&ctrtid=BPpOS)
   安装clipboard插件
	
	
实现方式四 vue-clipboard2插件 [参考](https://github.com/Inndy/vue-clipboard2) (最终选择的方案，亲测有效)
[详细关于粘贴板的api介绍参见](https://www.ruanyifeng.com/blog/2021/01/clipboard-api.html)
	
