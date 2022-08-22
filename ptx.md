### 1  Git 不小心在其他分支进行开发了：如何将内容提交到指定分支

> 未add和commit的 
  在当前分支内容暂存：Git stash save
  在要提交到的分支内容pop出来Git stash pop

> 已经commit的了 
  使用git log 获取commitid
  在要提交的分支利用git cherry-pick commitid 
  然后在git push 

在原有分支想放弃commit的内容 如何操作：

  A分支上修改的内容commit了，但是commit错了 想要在B分支上提交最后在merge到A分支上
  若利用cherry-pick 指定的commit到b分支 ，在将b分支的内容merge到a分支。a分支的commit也push同步了 会存在冲突么？？
  经测试不会存在冲突，但是在A分支上会多一个重复的commit,id不同：

### 2 项目npm时出现：npm WARN config global --global, --local are deprecated. Use --location=global instead.

这种提示只要将这两个文件中的"prefix -g"修改为"prefix --location=global",保存

### 3 新换了电脑运行npm install项目显示各种错误如下：
```
npm ERR! code ERESOLVE
npm ERR! ERESOLVE unable to resolve dependency tree
npm ERR! 
***
npm ERR! Could not resolve dependency:

***
```
输入npm -V发现我的npm版本为7.x的或者8*，因为npm7.x版本对某些命令比npm6.x更严格，所以莫名报了这个错
解决办法有两种：
1.在命令后加上--legacy-peer-deps
2.使用npm6.x
提示：使用npm@6不需要卸载npm@7。可以使用npx指定npm的版本。例如：npx -p npm@6 npm i --legacy-peer-deps
如果这不能立即起作用，可以先删除 node_modules和 package-lock.json

### 4 换电脑后项目扩展名问题

```
error Missing file extension for "@/api/login" import/extensions
error Missing file extension for "@/api/*" import/extensions
```
解决方法,在config增加如下配置：

```
const PATH = require('path');

configureWebpack: {
resolve: {
  alias: {
    "@": PATH.resolve(__dirname, './src')
  },
  extensions: ['.js', '.vue', '.json']
},
}
```

### 5 换电脑后项目部署存在问题 code exist

> 先删除了 package-lock.json 不起作用，又发现自己将node-module提到了测试环境导致了此次的失败；.gitignore 失效导致了node-module被提交！
> 把某些目录或文件加入忽略规则，按照上述方法定义后发现并未生效，原因是.gitignore只能忽略那些原来没有被追踪的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。那么解决方法就是先把本地缓存删除（改变成未被追踪状态），然后再提交：
```
git rm -r --cached .
git add .
git commit -m 'update .gitignore'
git push -u origin master
```
注意：
1. .gitignore只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。
2. 想要.gitignore起作用，必须要在这些文件不在暂存区中才可以，.gitignore文件只是忽略没有被staged(cached)文件， 对于已经被staged文件，加入ignore文件时一定要先从staged移除，才可以忽略。

### 6 常见的在文本框内插入其他文本功能
```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>练习 在input[type="textarea"]中获取光标位置并插入文本指定的文本内容</title>
</head>
<body>
  <div class="contentmenu-container">
    练习在input[type="textarea"]中获取光标位置并插入文本指定的文本内容
    <textarea id="textarea" name="tarea" placeholder="请输入文字" rows="" cols="" >
    </textarea>
    <button id='insert'>插入</button>
  </div>
  <script>
    var textareaEle = document.querySelector('#textarea')
    var insertEle = document.querySelector('#insert')
    textareaEle.addEventListener('select',function () {
        message.setCaret(this);
    })
    textareaEle.addEventListener('click',function () {
        message.setCaret(this);
    })
    textareaEle.addEventListener('keyup',function () {
        message.setCaret(this);
    });

    insertEle.addEventListener('click',function () {
        var textareaStr = textareaEle.innerHTML;
        message.insertAtCaret(textareaEle,'fgsfg fgsdfg fgsfg');
    });

    var message = {
      setCaret: function (textObj) {
        if (textObj.createTextRange) {
          textObj.caretPos = document.selection.createRange().duplicate();
        }
      },
      insertAtCaret: function (textObj, textFeildValue) {
        if (document.all) {
          console.log('caretPos', textObj.caretPos)
          if (textObj.createTextRange && textObj.caretPos) {
              var caretPos = textObj.caretPos;
              caretPos.text = caretPos.text.charAt(caretPos.text.length - 1) == ' ' ? textFeildValue + ' ' : textFeildValue;
            } else {
              textObj.value = textFeildValue;
            }
          } else {
            if (textObj.setSelectionRange) {
              var rangeStart = textObj.selectionStart;
              var rangeEnd = textObj.selectionEnd;
              console.log(rangeStart, rangeEnd)
              var tempStr1 = textObj.value.substring(0, rangeStart);
              var tempStr2 = textObj.value.substring(rangeEnd);
              textObj.value = tempStr1 + textFeildValue + tempStr2;
            } else {
              alert("This version of Mozilla based browser does not support setSelectionRange");
            }
        }
      }
    }
  </script>
</body>
</html>
```

### 7 vscode  shift+command+空格 安装code 已成功 再次重新开机失败了，报错如下：
```
EACCES: permission denied, unlink '/usr/local/bin/code'
```
没有找到具体的原因，但是可以通过先 uninstall 后在重新安装来解决



### 8 node 新电脑重新下载项目进行 npm install 时报错（node 版本造成的）
```
Cannot read properties of null (reading 'pickAlgorithm')
```
找到想要的方法说 删除package-lock.js里面的部分内容，但是删除后就报错如下：
```
npm ERR! code ERR_SOCKET_TIMEOUT
npm ERR! network Socket timeout
npm ERR! network This is a problem related to network connectivity.
npm ERR! network In most cases you are behind a proxy or have bad network settings.
npm ERR! network 
npm ERR! network If you are behind a proxy, please make sure that the
npm ERR! network 'proxy' config is set properly.  See: 'npm help config'

npm ERR! A complete log of this run can be found in:
****
```


[参考博文]https://blog.csdn.net/weixin_43865196/article/details/125254459
[2](https://juejin.cn/post/6844903794120065032)


### 7 nvm 用于管理node 版本 
[nvm github安装使用教程](https://github.com/nvm-sh/nvm)
```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
```
```
nvm install 12.20.0    比如安装12.20.0版本
nvu use 12.20.0

yarn  npm i yarn -g
yarn 相当于npm run的命令 


open . 打开当前目录

vi .zshrc. 编辑文件
source  .zshrc 运行一下文件


```
[参考文档1](https://www.jianshu.com/p/23775773b9d3?u_atoken=3c908cfc-0e1e-4059-b4fa-f95f1cfc1b91&u_asession=010UUUHpwbPb_ds7-E_H0Hui8GgYefpzGqgidJ4W0g5lFjDZ0XfqVexV1Th4fSupYbX0KNBwm7Lovlpxjd_P_q4JsKWYrT3W_NKPr8w6oU7K8TS2tIajr6C19emNs8t7VO4hmsJyv-1hubKihelhNJtGBkFo3NEHBv0PZUm6pbxQU&u_asig=05rxG5icSKLUFI70pJ_684hy08xbLSKzU5EUCDR5lqua2QqIykNodv4o8oFDoJb-SjUYzaeHTsNBcvDZjzvLCmrUw45l5XrG2VUDB76USyRAjDSba6Ev6H42HEInbckX68L_QzQOib4XWSmSFcwv-ZCw2YqiW9QMinlXpbIAEQzs79JS7q8ZD7Xtz2Ly-b0kmuyAKRFSVJkkdwVUnyHAIJzQXRkp3IBgq5KumN-vrgDoND0bST_iIJ5aqQPwpqol-M6vxqedUl-eZLCupCM_ImYu3h9VXwMyh6PgyDIVSG1W-Ju0OVKzP8mjRuJ7kYxdovMCiaxWNGs0SKRybKdVaVoenkgbGgHpPNQ4X84WCP2Y5BJFY80Xd36nlD_tkpDHUcmWspDxyAEEo4kbsryBKb9Q&u_aref=OuqFJ4Q4Ngd8wP59BmnW43sBHRE%3D)
[参考文档2](https://github.com/nvm-sh/nvm)



### 8  elementui 设置default-time 无效 
?


### 9 项目目录报错如下，类似问题4但是加上问题4的解决方案后仍然无效；
```
Missing file extension "vue" for


   "import/extensions": ["error", "always", {
      "js": "never",
      "vue": "never"
    }]
    
    
   "settings": {
    "import/resolver": {
      "alias": {
        "map": [
          ["@", "./src"]
        ],
        "extensions": [".vue", ".json", ".js"]
      }
    }
  }
  
  
之后报错变为Unexpected use of file extension "vue" for
最后将eslint改写为如下内容：
"rules": {
  "import/extensions": ["warn", "always", {
    "js": "never",
    "vue": "never"
  }]
}
  // "settings": {
  //   "import/resolver": {
  //     "alias": {
  //       "map": [
  //         ["@", "./src"]
  //       ],
  //       "extensions": [".vue", ".json", ".js"]
  //     }
  //   }
  // }
  在重新运行是可以了的
```
[extension相关文档](https://stackoverflow.com/questions/58671448/how-to-force-vue-extension-in-all-imports-using-eslint)

[VUE-CLI3解决Missing file extension "vue" for "xxxx" (import/extensions)](https://www.jianshu.com/p/fe96234d01cc)

[git缓冲问题导致](https://stackoverflow.com/questions/42308879/how-to-solve-npm-error-npm-err-code-elifecycle)

### 10 vue 报错如下
```
          <el-date-picker
            v-model="dateTime"
            type="daterange"
            :picker-options="pickerOptions"
            :range-separator="$t('common.to')"
            :start-placeholder="$t('order.startTime')"
            :end-placeholder="$t('order.endTime')"
            value-format="timestamp"
            :default-time="['00:00:00', '23:59:59']"
            align="right" />
            
vue.js?ba4c:5062 [Vue warn]: Avoid mutating a prop directly since the value will be overwritten whenever the parent component re-renders. Instead, use a data or computed property based on the prop's value. Prop being mutated: "placement"
found in
---> <ElDatePicker>

```


### 11 eslint语法检测中若有包引入顺序问题相关的报错 会导致code ELIFECYCLE；
```
09:25:57   ERROR  Failed to compile with 1 error上午1:25:56
09:25:57   error  in ./src/components/layout/TagsView.vue
09:25:57  Module Error (from ./node_modules/eslint-loader/index.js):
09:25:57  /data/jenkins/workspace/*/src/components/layout/TagsView.vue
09:25:57    45:27  warning  Missing file extension for "@/utils/i18n"                   import/extensions
09:25:57    46:1   error    `path` import should occur before import of `@/utils/i18n`  import/

09:25:57   ERROR  Build failed with errors.
09:25:57  npm ERR! code ELIFECYCLE
09:25:57  npm ERR! errno 1
09:25:57  npm ERR! urm@3.0.0 build: `vue-cli-service build`
09:25:57  npm ERR! Exit status 1
09:25:57  npm ERR! 
09:25:57  npm ERR! Failed at the urm@3.0.0 build script.
09:25:57  npm ERR! This is probably not a problem with npm. There is likely additional logging output above.
09:25:57  
09:25:57  npm ERR! A complete log of this run can be found in:
09:25:57  npm ERR!   /var/lib/jenkins/.npm/_logs/2022-08-15T01_25_56_891Z-debug.log
```
将包引入的顺序调换后就可以了


### 12 vscode code install后每次重启电脑会失效
前提先将vscode 拖拽到应用程序中在在终端运行如下命令；

```
sudo ln -fs "/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code" "/usr/local/bin/"
```

### 13 TypeError: undefined is not iterable (cannot read property Symbol(Symbol.iterator))

```
当进行结构赋值时，若date为非[]格式的内容就会报错如上：
    const [startT, endT] = this.date
    
    如下测试：
    const testvar = []
    const [a, b] = testvar
    console.log('a,b', a, b)// a,b undefined undefined

    const testvar1 = null
    const [a1, b1] = testvar1// "TypeError: testvar1 is not iterable"
    console.log('a1,b1', a1, b1)

    const testvar2 = undefined
    const [a2, b2] = testvar2// "TypeError: testvar2 is not iterable"
    console.log('a2,b2', a2, b2)

    // data 在data里面并没有被初始化
    const [a3, b3] = this.data//TypeError: undefined is not iterable (cannot read property Symbol(Symbol.iterator))
    console.log('a3,b3', a3, b3)
```


























