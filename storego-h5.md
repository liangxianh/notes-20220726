### 基于vue vant 的部分商城类的项目

一 项目构建从无到有
```
npm install -g vue-cli(没反应，可以修改一下镜像)(vue-cli 新版本已经更新了名字@vue/cli)
npm config set registry http://registry.npm.taobao.org/
npm install -g @vue/cli

安装其他包，注意
-D  --save-dev。包将保存在devDependencies
-S  --save 包将保存在dependencies

注意vue和vue-router版本需要对应使用
使用vue2安装对应的router3版本;
使用vue3安装对应的router4版本;
```

二 遇到的问题总结

1. ivue-router 使用报错 Uncaught TypeError: Cannot read properties of undefined (reading 'install')

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
