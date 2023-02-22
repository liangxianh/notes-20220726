### 正则表达式

> 1 魅力

如一下提取字符串里的数字,正则一句话搞定；可以提升代码质量；

```
    let str = "ksdjfaku934kdjf93840sdfo"
    // 现在想要提取上面str里面的数字
    let nums = [...str].filter(a => !Number.isNaN(parseInt(a)))
    console.log(nums.join(''))

    console.log(str.match(/\d/g).join(''))
```

> 2 创建

* 字面量创建，其由包含在斜杠之间的模式组成

```
const re = /\d+/g;
```

* 调用RegExp对象的构造函数

```
const re = new RegExp("\\d+","g");

const rul = "\\d+"
const re1 = new RegExp(rul,"g");

// 
    let con = prompt('请输入要检查的内容， 支持正则')
    let reg = new RegExp(con, 'g')

    let divcontent = document.querySelector('div')
    divcontent.innerHTML = divcontent.innerHTML.replace(reg, search => {
      return `<span style="color: red;">----${search}---</span>`
    })
```
使用构建函数创建，第一个参数可以是一个变量，**遇到特殊字符\需要使用\\进行转义**


> 3 m 进行单行处理

```
 let hd2 = `
      #1 js,200元 #
      #3 java,300元 #
      #12 php,200元 #
      #32 regtest.com # homkdjfk
      #33 node.js,200元 #
    `
    let lessons = hd2.match(/^\s*#\d+\s+.+\s+#$/gm).map(v => {
      v = v.replace(/\s*#\d+\s*/, '').replace(/\s*#/, '')
      console.log(v)
      name = v.split(',')[0]
      price = v.split(',')[1]
      // [name, price] = v.split(',')
      return { name, price }
    })
    console.log(JSON.stringify(lessons, null, 2))
```

**注意：replace(/\s*#/, '') 这里的正则表达式不要加引号**

> 4 汉字与字符属性

```

```
| 属性字母 | 代表含义 | 属性字母 中文翻译 |
| ---| ---| ---|
| C | Other | C 其他 | 
| Cc | Control | Cc 控制 | 
| Cf | Format | Cf 格式 | 
| Cn | Unassigned | Cn 未分配的 | 
| Co | Private use | Co 私人使用 | 
| Cs | Surrogate | Cs 代理人 | 
| L | Letter	Includes the following properties: Ll, Lm, Lo, Lt and Lu. | L 字母 包括以下属性。Ll, Lm, Lo, Lt 和 Lu。| 
| Ll | Lower case letter | Ll 小写字母 | 
| Lm | Modifier letter | Lm 修饰语字母 | 
| Lo | Other letter | Lo 其他字母 | 
| Lt | Title case letter | Lt 标题字母 | 
| Lu | Upper case letter | Lu 大写字母 | 
| M | Mark | M 标记 | 
| Mc | Spacing mark | Mc 间隔标记 | 
| Me | Enclosing mark | Me 包容性标记 | 
| Mn | Non - spacing mark | Mn 非间距标记 | 
| N | Number | N 数字 | 
| Nd | Decimal number | Nd 十进制数字 | 
| Nl | Letter number | Nl 字母编号 | 
| No | Other number | No 其他数字 | 
| P | Punctuation | P 标点符号 | 
| Pc | Connector punctuation | Pc 连字符标点符号 | 
| Pd | Dash punctuation | Pd 破折号标点符号 | 
| Pe | Close punctuation | Pe 关闭标点符号 | 
| Pf | Final punctuation | Pf 最后的标点符号 | 
| Pi | Initial punctuation | Pi 首字母标点符号 | 
| Po | Other punctuation | Po 其他标点符号 | 
| Ps | Open punctuation | Ps 开放式标点符号 | 
| S | Symbol | S 符号 | 
| Sc | Currency symbol | Sc 货币符号 | 
| Sk | Modifier symbol | Sk 修饰符号 | 
| Sm | Mathematical symbol | Sm 数学符号 | 
| So | Other symbol | So 其他符号 | 
| Z | Separator | Z 分隔符 | 
| Zl | Line separator | Zl 行分隔符 | 
| Zp | Paragraph separator | Zp 段落分隔符 | 
| Zs | Space separator | Zs 空格分隔符 | 


| 属性字母 | 代表含义 | 属性字母 中文翻译 |
| ---| ---| ---|
｜C  ｜	Other  ｜ C 其他｜
｜Cc	｜Control  ｜ Cc 控制｜
｜Cf	｜Format  ｜ Cf 格式｜
｜Cn	｜Unassigned  ｜ Cn 未分配的｜
｜Co	｜Private use  ｜ Co 私人使用｜
｜Cs	｜Surrogate  ｜ Cs 代理人｜
｜L	 ｜ Letter	Includes the following properties: Ll, Lm, Lo, Lt and Lu. ｜ L 字母 包括以下属性。Ll, Lm, Lo, Lt 和 Lu。｜
｜Ll	｜Lower case letter  ｜ Ll 小写字母｜
｜Lm	｜Modifier letter  ｜ Lm 修饰语字母｜
｜Lo	｜Other letter  ｜ Lo 其他字母｜
｜Lt	｜Title case letter  ｜ Lt 标题字母｜
｜Lu	｜Upper case letter  ｜ Lu 大写字母｜
｜M	 ｜ Mark  ｜ M 标记｜
｜Mc	｜Spacing mark  ｜ Mc 间隔标记｜
｜Me	｜Enclosing mark  ｜ Me 包容性标记｜
｜Mn	｜Non - spacing mark  ｜ Mn 非间距标记｜
｜N	 ｜ Number  ｜ N 数字｜
｜Nd	｜Decimal number  ｜ Nd 十进制数字｜
｜Nl	｜Letter number  ｜ Nl 字母编号｜
｜No	｜Other number  ｜ No 其他数字｜
｜P	 ｜ Punctuation  ｜ P 标点符号｜
｜Pc	｜Connector punctuation  ｜ Pc 连字符标点符号｜
｜Pd	｜Dash punctuation  ｜ Pd 破折号标点符号｜
｜Pe	｜Close punctuation  ｜ Pe 关闭标点符号｜
｜Pf	｜Final punctuation  ｜ Pf 最后的标点符号｜
｜Pi	｜Initial punctuation  ｜ Pi 首字母标点符号｜
｜Po	｜Other punctuation  ｜ Po 其他标点符号｜
｜Ps	｜Open punctuation  ｜ Ps 开放式标点符号｜
｜S	 ｜ Symbol  ｜ S 符号｜
｜Sc	｜Currency symbol  ｜ Sc 货币符号｜
｜Sk	｜Modifier symbol  ｜ Sk 修饰符号｜
｜Sm	｜Mathematical symbol  ｜ Sm 数学符号｜
｜So	｜Other symbol  ｜ So 其他符号｜
｜Z	 ｜ Separator  ｜ Z 分隔符｜
｜Zl	｜Line separator  ｜ Zl 行分隔符｜
｜Zp	｜Paragraph separator  ｜ Zp 段落分隔符｜
｜Zs	｜Space separator  ｜ Zs 空格分隔符｜
[字符属性参考文献](https://tool.oschina.net/uploads/apidocs/php-zh/regexp.reference.unicode.html)



