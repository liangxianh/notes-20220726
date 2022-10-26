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
