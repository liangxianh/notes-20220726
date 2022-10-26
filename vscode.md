#### 1 vscode里面编辑vue文件 使用less时 在less代码中使用command+/注释发现仍然是html的注释<!-- --> 
```
发现在vscode setting.json里面设置如下关联文件
 "files.associations": {
    "*.vue": "vue" // less可正常显示只是注释是<!-- -->  不指定的话默认是这个
    "*.vue": "html" // less语法将报错只支持css的语法 command+/注释是 /*  */
    "*.vue": "vue-html"  // sccript和less都不可正常显示只是注释是<!-- --> 
  },
```
 vscode插件安装的问题导致的
 将其他插件都删除只保留如下的插件就可以了
 
![image](https://user-images.githubusercontent.com/31762176/197996151-b362b641-9a44-49c5-baee-aac47b3049bd.png)

#### 2 setting.json
```
{
  "workbench.startupEditor": "none",
  "workbench.colorTheme": "One Dark Pro",
  "editor.fontSize": 12,
  "editor.autoClosingQuotes": "always",
  "editor.bracketPairColorization.independentColorPoolPerBracketType": true,
  "editor.rename.enablePreview": false,
  "workbench.editor.enablePreview": false,
  "editor.wordWrap": "on",
  "eslint.codeAction.showDocumentation": {
    "enable": true
  },
  "editor.detectIndentation": false,
  "editor.tabSize": 2,
  // "vetur.format.defaultFormatter.html": "js-beautify-html",
  "vetur.format.defaultFormatterOptions": {
    "js-beautify-html": {
      // "wrap_line_length": 90,
      // "wrap_attributes": "force",
      "end_with_newline": false
    }
  },
  "vetur.format.defaultFormatter.js": "vscode-typescript",
  "files.eol": "auto",
  "[javascript]": {
    // "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[vue]": {
    // 可选值： eslint ："dbaeumer.vscode-eslint"  vetur: "octref.vetur"   prettier: "esbenp.prettier-vscode"
    // 对 vue 文件，使用 prettier（格式化规则） + eslint（校验） 进行格式化，也可以选择 vetur 插件，或者单独选择prettier不加eslint
    // "editor.defaultFormatter": "octref.vetur"
  },
  "[html]": {
    "editor.defaultFormatter": "vscode.html-language-features"
  },
  "[json]": {
    "editor.defaultFormatter": "vscode.json-language-features"
  },
  "[jsonc]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescript]": {
    // "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "eslint.run": "onSave",
  "eslint.options": {
    "plugin": ["html"],
    "extensions": [".js", ".vue"]
  },
  "eslint.validate": [
    "javascriptreact",
    "vue",
    "javascript",
    {
      "language": "vue",
      "autoFix": true
    },
    {
      "language": "html",
      "autoFix": true
    }
  ],
  "editor.codeActionsOnSave": {
  
    "source.fixAll.eslint": true
  },
  "eslint.format.enable": true,
  "editor.formatOnSave": true, // 格式化 html
  "fileheader.customMade": {
    // 头部注释默认字段
    "Author": "xianhong.liang",
    "Date": "Do not edit", // 设置后默认设置文件生成时间
    "LastEditTime": "Do not edit", // 设置后，保存文件更改默认更新最后编辑时间
    "LastEditors": "xianhong.liang", // 设置后，保存文件更改默认更新最后编辑人
    "Description": "",
    "FilePath": "Do not edit", // 设置后，默认生成文件相对于项目的路径
    "custom_string_obkoro1": ""
  },
  "fileheader.configObj": {
    "autoAdd": true, // 检测文件没有头部注释，自动添加文件头部注释
    "autoAddLine": 10000, // 文件超过多少行数 不再自动添加头部注释
    "autoAlready": true, // 只添加插件支持的语言以及用户通过`language`选项自定义的注释
    "supportAutoLanguage": ["html", "vue", "js", "css", "less", "scss"] // 设置之后，在数组内的文件才支持自动添加
  },
  "security.workspace.trust.untrustedFiles": "open",
  // 给files.associations对象添加需要补齐代码的文件格式
  "files.associations": {
    "*.wml": "html",
    "*.wxss": "css",
    "*.wxml": "vue-html"
    // "*.vue": "vue"
  },
  "editor.suggestSelection": "first",
  "vsintellicode.modify.editor.suggestSelection": "automaticallyOverrodeDefaultValue",
  "eslint.workingDirectories": [],
  "editor.rulers": [],
  "diffEditor.wordWrap": "on",
  "eslint.nodeEnv": "",
  "files.autoSave": "afterDelay",
  "editor.language.colorizedBracketPairs": [],
  "gitlens.currentLine.dateFormat": "",
  "gitlens.codeLens.dateFormat": "",
  "editor.semanticTokenColorCustomizations": {},
  "editor.fontLigatures": false
}

```
