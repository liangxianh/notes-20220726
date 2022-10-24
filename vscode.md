#### 1 vscode里面编辑vue文件 使用less时 在less代码中使用command+/注释发现仍然是html的注释<!-- --> 
```
发现在vscode setting.json里面设置如下关联文件
 "files.associations": {
    "*.vue": "vue" // less可正常显示只是注释是<!-- -->  不指定的话默认是这个
    "*.vue": "html" // less语法将报错只支持css的语法 command+/注释是 /*  */
    "*.vue": "vue-html"  // sccript和less都不可正常显示只是注释是<!-- --> 
  },
```
 
