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











