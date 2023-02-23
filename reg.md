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
let hd = 'regtesst20230222, 传音人'
// 匹配字母
console.log(hd.match(/\p{L}/ug))
// 匹配标点符号
console.log(hd.match(/\p{P}/ug))
// 匹配中文
console.log(hd.match(/\p{sc=Han}/ug))
```
* Unicode 属性 \p{…}

Unicode 中的每个字符都有很多属性。它们描述了字符所属的“类别”，包含了关于字符的各种信息。

例如，如果一个字符具有 Letter 属性，这意味着这个字符归属于（任意语言的）字母表。而 Number 属性则表示这是一个数字：也许是阿拉伯数字，亦或是中文数字，等等。

我们可以查找具有某种属性的字符，写作 \p{…}。为了使用 \p{…}，一个正则表达式必须使用修饰符 u。

举个例子，\p{Letter} 表示任何语言中的一个字母。我们也可以使用 \p{L}，因为 L 是 Letter 的一个别名。对于每种属性而言，几乎都存在对应的缩写别名。

在下面的例子中会找出来 3 种字母：英语、格鲁吉亚语和韩语。
```
let str = "A ბ ㄱ";
alert( str.match(/\p{L}/gu) ); // A,ბ,ㄱ
alert( str.match(/\p{L}/g) ); // null（没有匹配项，因为没有修饰符 "u"）
```
以下是主要的字符类别和它们对应的子类别：

字母（Letter）L：
小写（lowercase）Ll，
修饰（modifier）Lm，
首字母大写（titlecase）Lt，
大写（uppercase）Lu，
其它（other）Lo。
数字（Number）N：
十进制数字（decimal digit）Nd，
字母数字（letter number）Nl，
其它（other）No。
标点符号（Punctuation）P：
连接符（connector）Pc，
横杠（dash）Pd，
起始引号（initial quote）Pi，
结束引号（final quote）Pf，
开（open）Ps，
闭（close）Pe，
其它（other）Po。
标记（Mark）M（accents etc）：
间隔合并（spacing combining）Mc，
封闭（enclosing）Me，
非间隔（non-spacing）Mn。
符号（Symbol）S：
货币（currency）Sc，
修饰（modifier）Sk，
数学（math）Sm，
其它（other）So。
分隔符（Separator）Z：
行（line）Zl，
段落（paragraph）Zp，
空格（space）Zs。
其它（Other）C：
控制符（control）Cc，
格式（format）Cf，
未分配（not assigned）Cn，
私有（private use）Co，
代理伪字符（surrogate）Cs。
因此，比如说我们需要小写的字母，就可以写成 \p{Ll}，标点符号写作 \p{P} 等等。

也有其它派生的类别，例如：

Alphabetic（Alpha），包含了字母 L，加上字母数字 Nl（例如 Ⅻ —— 罗马数字 12），加上一些其它符号 Other_Alphabetic（OAlpha）。
Hex_Digit 包括 16 进制数字 0-9，a-f。
……等等。
Unicode 支持很多不同的属性，列出整个清单需要占用大量的篇幅，因此在这里列出相关的链接：
[列出一个字符的所有属性：](https://unicode.org/cldr/utility/character.jsp)
[按照属性列出所有的字符：](https://unicode.org/cldr/utility/list-unicodeset.jsp)
[属性的对应缩写形式：](https://www.unicode.org/Public/UCD/latest/ucd/PropertyValueAliases.txt)
[以文本格式整理的所有 Unicode 字符，包含了所有的属性：](https://www.unicode.org/Public/UCD/latest/ucd/)
|支持的Unicode属性|
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

指定大小写不敏感匹配对这些转义序列不会产生影响，比如， \p{Lu} 始终匹配大写字母。

Unicode 字符集在具体文字中定义。使用文字名可以匹配这些字符集中的一个字符。例如：

```
\p{Greek}
\P{Han} 
```

[字符属性参考文献](https://tool.oschina.net/uploads/apidocs/php-zh/regexp.reference.unicode.html)
[字符属性参考文献2](https://www.php.net/manual/zh/regexp.reference.unicode.php)


