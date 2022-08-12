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
configureWebpack: {
resolve: {
  alias: {
    "@": path.resolve(__dirname, './src')
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




























