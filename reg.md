### 正则表达式

> 1 魅力及常见规则

如一下提取字符串里的数字,正则一句话搞定；可以提升代码质量；

```
    let str = "ksdjfaku934kdjf93840sdfo"
    // 现在想要提取上面str里面的数字
    let nums = [...str].filter(a => !Number.isNaN(parseInt(a)))
    console.log(nums.join(''))

    console.log(str.match(/\d/g).join(''))
```
常见的校验规则如下：
| 规则	| 描述 |
| ---  | --- |
| \ | 转义 |
| ^ | 匹配输入的开始 |
| $ | 匹配输入的结束 |
| * | 匹配前一个表达式 0 次或多次 |
| + | 匹配前面一个表达式 1 次或者多次。等价于 {1,} |
| ? | 匹配前面一个表达式 0 次或者 1 次。等价于{0,1} |
| . | 默认匹配除换行符之外的任何单个字符 |
| x(?=y) | 匹配'x'仅仅当'x'后面跟着'y'。这种叫做先行断言 |
| (?<=y)x	| 匹配'x'仅当'x'前面是'y'.这种叫做后行断言 |
| x(?!y)	| 仅仅当'x'后面不跟着'y'时匹配'x'，这被称为正向否定查找 |
| (?<!y)x	| 仅仅当'x'前面不是'y'时匹配'x'，这被称为反向否定查找 |
| x|y	| 匹配‘x’或者‘y’ |
| {n}	n | 是一个正整数，匹配了前面一个字符刚好出现了 n 次 |
| {n,}	| n是一个正整数，匹配前一个字符至少出现了n次 |
| {n,m}	| n 和 m 都是整数。匹配前面的字符至少n次，最多m次 |
| [xyz]	| 一个字符集合。匹配方括号中的任意字符 |
| [^xyz] | 匹配任何没有包含在方括号中的字符 |
| \b |	匹配一个词的边界，例如在字母和空格之间 |
| \B |	匹配一个非单词边界 |
| \d |	匹配一个数字 |
| \D |	匹配一个非数字字符 |
| \f |	匹配一个换页符 |
| \n |	匹配一个换行符 |
| \r |	匹配一个回车符 |
| \s |	匹配一个空白字符，包括空格、制表符、换页符和换行符 |
| \S |	匹配一个非空白字符 |
| \w |	匹配一个单字字符（字母、数字或者下划线） |
| \W |	匹配一个非单字字符 |


正则表达式标记
 | 标志 | 描述 | 
 | --- | --- |
 | g | 全局搜索 | 
 | i | 不区分大小写搜索 | 
 | m | 多行搜索 | 
 | s | 允许 . 匹配换行符 | 
 | u | 使用unicode码的模式进行匹配 | 
 | y | 执行“粘性(sticky)”搜索,匹配从目标字符串的当前位置开始 |
 
匹配方法

* 字符串（str）方法：match、matchAll、search、replace、split
* 正则对象下（regexp）的方法：test、exec
* 
| 方法 | 描述 |
| ---| --- |
| exec	| 一个在字符串中执行查找匹配的RegExp方法，它返回一个数组（未匹配到则返回 null） |
| test	| 一个在字符串中测试是否匹配的RegExp方法，它返回 true 或 false |
| match	| 一个在字符串中执行查找匹配的String方法，它返回一个数组，在未匹配到时会返回 null |
| matchAll	| 一个在字符串中执行查找所有匹配的String方法，它返回一个迭代器（iterator） |
| search	| 一个在字符串中测试匹配的String方法，它返回匹配到的位置索引，或者在失败时返回-1 |
| replace	| 一个在字符串中执行查找匹配的String方法，并且使用替换字符串替换掉匹配到的子字符串 |
| split	| 一个使用正则表达式或者一个固定字符串分隔一个字符串，并将分隔后的子字符串存储到数组中的 String 方法 |

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
```
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
```
因此，比如说我们需要小写的字母，就可以写成 \p{Ll}，标点符号写作 \p{P} 等等。

也有其它派生的类别，例如：

Alphabetic（Alpha），包含了字母 L，加上字母数字 Nl（例如 Ⅻ —— 罗马数字 12），加上一些其它符号 Other_Alphabetic（OAlpha）。
Hex_Digit 包括 16 进制数字 0-9，a-f。
……等等。
Unicode 支持很多不同的属性，列出整个清单需要占用大量的篇幅，因此在这里列出相关的链接：

* [正则表达式之Unicode：修饰符 "u" 和类 \p{...}](https://www.cnblogs.com/lxlx1798/articles/16975828.html)
* [列出一个字符的所有属性：](https://unicode.org/cldr/utility/character.jsp)
* [按照属性列出所有的字符：](https://unicode.org/cldr/utility/list-unicodeset.jsp)
* [属性的对应缩写形式：](https://www.unicode.org/Public/UCD/latest/ucd/PropertyValueAliases.txt)
* [以文本格式整理的所有 Unicode 字符，包含了所有的属性：](https://www.unicode.org/Public/UCD/latest/ucd/)
* [Unicode的script取值]（https://en.wikipedia.org/wiki/Script_(Unicode)）


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

> 5 u(Unicode)

JavaScript 对字符串使用 Unicode 编码。大多数字符使用 2 个字节编码，但这种方式只能编码最多 65536 个字符。

这个范围不足以对所有可能的字符进行编码，这就是为什么使用 4 个字节对一些罕见的字符进行编码，比如 𝒳（数学符号 X）或 😄（笑脸），一些象形文字等等。

下面是一些字符对应的 Unicode 值：

|字符	|Unicode	|Unicode 中的字节数|
|---|---|------|
|a|	0x0061	|2|
|≈|	0x2248	|2|
|𝒳| 0x1d4b3 |4|
|𝒴	|0x1d4b4 |4|
|😄| 0x1f604 |4|
所以像 a 和 ≈ 这样的字符占用 2 个字节，而 𝒳，𝒴 和 😄 的对应编码则更长，占用 4 个字节。

很久以前，当 JavaScript 被发明出来的时候，Unicode 编码要更加简单：当时没有 4 个字节的字符。所以，有些语言功能现在仍无法正确处理它们。

比如 length 认为这里有 2 个字符：
```
alert('😄'.length); // 2
alert('𝒳'.length); // 2
```
……但我们可以清楚地认识到这里只有一个字符，对吧？关键在于 length 把 4 个字节当成了 2 个 2 字节长的字符。这是不对的，因为它们必须被当作一个整体来考虑（即所谓的“代理对（surrogate pair）”，你可以在 字符串 中阅读关于代理对的更多信息）。

默认情况下，正则表达式也会把一个 4 个字节的“长字符”当成一对 2 个字节长的字符。正如在字符串中遇到的情况，这将导致一些奇怪的结果。。

与字符串有所不同的是，正则表达式有一个修饰符 u 被用以解决此类问题。当一个正则表达式带有这个修饰符后，4 个字节长的字符将被正确地处理。同时也能够使用 Unicode 属性进行查找了。

[参考文章1,字符](https://zh.javascript.info/string)


> 6 原子表原子组

* 原子表： []
* 原子组：()      \1代表reg中的第一个原子组  \2代表reg中的第一个原子组

```
    let datr = '2023-09-09'
    let reg3 = /^\d{4}([-\/])\d{2}\1\d{2}$/
    console.log(datr.match(reg3)) // ['2023-09-09', '-', index: 0, input: '2023-09-09', groups: undefined]

    let reg4 = /^(\d{4})([-\/])\d{2}\2\d{2}$/
    console.log(datr.match(reg4)) // ['2023-09-09', '2023', '-', index: 0, input: '2023-09-09', groups: undefined]
```

> 7  原子组[]中的符号就是单纯的符号；
 
 ```
    let str5 = "jfkakdf.kjk)()"
    let reg5 = /[().+]/g
    console.log(str5.match(reg5)) //(4) ['.', ')', '(', ')']
```

> 8 不记录原子组(?:reg)

```
    // 原子组只是想匹配，不想输出，直接在原子组前面?:
    let str888 = `
    https://www.baidu.com
    http://houdunren.com
    `

    let reg888 = /https?:\/\/((?:\w+\.)?\w+\.(?:com|cn|org|cc))/ig
    console.log(str888.match(reg888))

    console.log(reg888.exec(str888))
    // 有g时才会继续向后查询； 无g时lastindex 每次从0开始
    console.log(reg888.exec(str888))
    console.log(reg888.exec(str888))

    console.log('---------')
    console.log(str888.replace(reg888, `$1$2`)) 
    //www.baidu.com$2  不记录顾$2没有只被作为单纯的字符串；
    // houdunren.com$2
```


> 9 多个正则用于同一个字符串验证，比如密码验证

```

  <div class="content">
    <input type="text" name="password">
  </div>
  <script>
    // 批量使用reg完成验证密码
    const input = document.querySelector(`[name="password"]`)
    input.addEventListener('keyup', e => {
      const value = e.target.value
      console.log(value)
      const regs = [
        /^[a-z0-9]{5,10}$/i,
        /[A-Z]/,
        /[a-z]/,
        /[0-9]/,
      ]
      let state = regs.every(e => e.test(value))
      console.log(state ? 'right' : 'wrong')
    })
   </script>
```

> 10 禁止贪婪，在需要禁止的地方加？

以标签替换为例
```
  <div class="content">
    <main>
      <span>kdfjk.com</span>
      <span>kdjf;al.co,m</span>
      <span>houdunren.com</span>
    </main>
  </div>
  <script>
    // 将span--》h4 且描红，加前缀fight!!!-
    const main = document.querySelector('main')
    const reg = /<span>([\s\S]+)<\/span>/gi
    const reg22 = /<span>([\s\S]+?)<\/span>/gi
    // main.innerHTML = main.innerHTML.replace(reg, (v, p1) => {
    //   console.log(v) // 这种情况会贪婪匹配，结果如下: 包括空格
    //   /* 
    //   //只有一个结果
    //   <span>kdfjk.com</span>
    //      <span>kdjf;al.co,m</span>
    //      <span>houdunren.com</span>
    //   */
    // })
    // 加上？就会尽量偏向少的那面；
    main.innerHTML = main.innerHTML.replace(reg22, (v, p) => {
      // console.log(v) //这样会匹配3个结果
      return `<h4 style="color: red">fight!!!-${p}</h4>`
    })
```


11 matchAll（同match一样也是字符串方法）；

```
    let regMatch = /<(h[1-6])>([\s\S]+?)<\/\1>/i;
    let regMatchg = /<(h[1-6])>([\s\S]+?)<\/\1>/ig;
    const headerh = document.querySelector('header')

    // 使用match时存在问题：reg+g 则匹配不到内容，不加g则只能匹配到第一个；
    console.log(headerh.innerHTML.match(regMatch))
    console.dir(headerh.innerHTML.match(regMatchg))
```

![image](https://user-images.githubusercontent.com/31762176/222385032-71c6c5ba-8999-45b3-a229-4d3695fc226d.png)

针对match的问题可以使用matchAll来处理，但matchAll 有兼容性问题，部分低版本浏览器存在问题；
```
    const iteratorI = headerh.innerHTML.matchAll(regMatchg)
    let contents = [];
    for (let item of iteratorI) {
      console.log(item)
      contents.push(item[2])
    }
    console.log(contents)
```
可以兼容低版本浏览器自己实现matchAll方法


> 12 正则方法exec（）


> 13 $符在正则替换中的作用；

  * $&：代表匹配到的内容
  * $`: 匹配内容左边（前）的内容
  * $': 匹配内容右边（后）边的内容

```
let hd = "%hou="
   let hd2 = "%hou=houkkk33"
   let reg = /hou/g
   console.log(hd.replace(reg, "$&$&"))  // %houhou=
   console.log(hd.replace(reg, "$'"))    /// %==
   console.log(hd.replace(reg, "$`z$`d"))  //  %z%d=   注意$`在首位或者在连续的两个第一个总是不被识别而忽略
   console.log(hd.replace(reg, " $`z$`d"))   // % %z%d=
   console.log(hd.replace(reg, " $`$`z$`d"))   // % %z%d=
   console.log(hd.replace(reg, " $`$`$`z$`d"))  //% %%z%d=
   // 实例
   const main = document.querySelector('body main')
   main.innerHTML = main.innerHTML.replace(/百度/, `<a href="https://www.baidu.com">$&</a`)
```



> 14 原子组在替换中的使用技巧

```
// 可以批量接受原子组参数，然后
```

> 15 原子组的别名(?<link>reg) 

以获取页面内a链接的href和title为例；

```
html body内
    <main>
      <a target=“blank” href="http://www.baidu.com">百度</a>
      <a id="k" href="http://www.kkk.com">kkk</a>
      <a id="k" href="asd.com">kkk</a>
      <a id="k" href="asdfkk.com">kkk</a>
      <a href="http://google.com">google</a>
    </main>
    js
    // <a后面.*?禁止贪婪匹配任意字符，
    // (?<link>.*?)单引号或者双引号中间的任意字符禁止贪婪
    const reg = /<a.*?href=(['"])(?<link>.*?)\1>(?<title>.*?)<\/a>/ig
    // console.log(main.innerHTML.match(reg))  //可以reg不带g测试看下输出格式，里面有参数groups，包含定义的组的别名和匹配的值信息；
    const links = []
    for(const iterator of main.innerHTML.matchAll(reg)) {
      links.push(iterator["groups"])
    }
    console.log(links)
    // 只提取带http（s）的
    const reg2 = /<a.*?href=(['"])(?<link>https?:\/\/(.*?))\1>(?<title>.*?)<\/a>/ig
    const links2 = []
    for(const iterator2 of main.innerHTML.matchAll(reg2)) {
      links2.push(iterator2["groups"])
    }
    console.log(links2)
```
![image](https://user-images.githubusercontent.com/31762176/223060869-8133c5cd-d48d-47d8-9e6d-577ad18bbbb3.png)

> 16 断言

* reg(?=content)后面断言匹配
* reg(?!contentreg)后面非断言
* (?<=content)reg前面断言匹配 ()
* (?<!contentreg)后行断言，前面不是contentreg的内容

后面断言规范价格为例
```
let lessons = `
      js, 200元,300次
      java, 300.00元, 34次
      node.js, 250元,230次
      php, 99元,30次
    `
    let reg = /(\d+)(.00)?(?=元)/gi
    // for(let iter of lessons.matchAll(reg)){
    //   console.log(iter)
    // }
    // console.log('=======')
    lessons = lessons.replace(reg, (v, ...args) => {
      // 注意进入的都是匹配到的内容
      // console.log(args) // 注意此出打印的是下面处理splice之后的结果，splice会影响原数组
      args[1] = args[1] || '.00'
      const temp =  args.splice(0,2).join("")
      // console.log('temp', temp)
      return temp
    })
    console.log(lessons)
```

后面非断言
```
   let strnotif = "houdun3432hmcl"
    let regnofif = /[a-z]+(?!\d)$/g
    // const regbif = /[a-z]+(?!\d+)$/g
    console.log(strnotif.match(regnofif)) // ['hmcl']   //不加$houdu 会被贪婪匹配到n之前
```

前面断言以模糊手机号为例
```
    let user = `
      张三: 15988989876
      里斯: 27688987656
    `
    const regbif = /(?<=\d{7})\d{4}/g
    user = user.replace(regbif, (v) => {
      return '*'.repeat(4)
    })
    console.log(user)
```
用户名不包含某个字段
```
    const input = document.querySelector('[name="username"]')
    input.addEventListener('keyup', function(){
      // (?!.*凉糕.*)限制条件从开始到结束都不包含 凉糕
      const reg = /^(?!.*凉糕.*).*/i
      console.log(this.value.match(reg))
    })
```



