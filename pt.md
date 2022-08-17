### 1 布尔型数据存储到cookie或者session中会变为字符串类型的数据，需要将内容进行转换一下在使用

* js常常用到将数据存储到<b>cookie</b>和<b>session</b>中，或者从<b>客户端的方法中获取数据</b>，一定注意获取的对象或者数据是字符串类型的，使用之前记得进行转换，可以使用JSON.parse（）

```
    Cookies.set('t', false)
    const tttt = Cookies.get('t')
    console.log('tttt', tttt, tttt && true, JSON.parse(tttt) && true)

    localStorage.setItem('tt', false)
    const lsVars = localStorage.getItem('tt')
    console.log('lsVars', lsVars, lsVars && true, JSON.parse(lsVars) && true)

    // tttt false true false
    // lsVars false true false
```


### 2 浏览器突然就不能newwork和console了，什么内容都看不到

* 可能原因是newwork被过滤了，删除过滤条件即可
* 但是console的内容即使恢复设置也不行，还没找到具体的原因？？？
