### 1 code ELIFECYCLE
npm ERR! errno 1
npm ERR! webpack-docs@1.0.0 start: `vuepress dev docs`
解决方案
```
rm -rf node_modules
rm -rf package-lock.json
npm cache clear --force
npm install
```
但是有的具体的代码冲突不能被解决,有的部分原因是node版本问题
原因
Node版本太低，我当时Node版本为12，运行就报上面错误；将Node版本升级到v16.13.0之后就正常运行了

解决方法
升级Node版本
```
Error [ERR_PACKAGE_PATH_NOT_EXPORTED]: Package subpath './dist/vue-router.esm-bundler.js' is not defined by "exports" in /Users/liangxianhong/Desktop/myprivate/study/webpack_docs/node_modules/vue-router/package.json
    at applyExports (internal/modules/cjs/loader.js:492:9)
```
