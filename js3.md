### js 数据类型

* 基本数据类型 Number String Boolean Undefined null symbol
*引用数据类型 object array function 
*其他引用数据类型 Date Regexp map set等 

  Date RegExp对象赋值规则同基本类型一致  Map Set对象赋值规则同引用类型一致
  
```
// 基本数据类型 symbol 
    let info1 = {
      name: '婷婷',
      age: 24,
      job: '公司前台',
      [Symbol('description')]: '平时喜欢做做瑜伽，人家有男朋友，你别指望了',
      [Symbol(43)]: 34
    }
    let info2 = {
      [Symbol('description')]: '这小姑娘挺好的，挺热情的，嘿嘿嘿……',
      [Symbol('foo')]: 'foo'
    }
    let target = {};
    Object.assign(target, info1, info2);
    console.log(target)
    for (var i in target) {
      console.log(i);   // 输出 "c" 和 "d"
    }

    console.log(Object.getOwnPropertySymbols(target))

    const symbol1 = Symbol();
    const symbol2 = Symbol(42);
    const symbol3 = Symbol('foo');

    console.log(typeof symbol1);
    // Expected output: "symbol"

    console.log(symbol2 == 42);
    // Expected output: false

    console.log(symbol3.toString());
    // Expected output: "Symbol(foo)"

    console.log(Symbol('foo') === Symbol('foo'));
    // Expected output: false

    console.log(Object(Symbol('foo')) == 'foo')
    //  Expected output: false

    console.log(Object('foo') == 'foo')
    //  Expected output: true


    // 引用数据类型 object array function 
    // 其他引用数据类型 Date Regexp map set等 (Date RegExp对象赋值规则同基本类型一致  Map Set对象赋值规则同引用类型一致)
    let date1 = new Date('2023-10-1 00:00:00')
    let date2 = date1
    console.log('date2 ', date2)
    data1 = new Date('2024-02-8 00:00:00')
    console.log('date2', date2)
    console.log('date1', data1)
    // date2  Sun Oct 01 2023 00:00:00 GMT+0800 (中国标准时间)
    // date2 Sun Oct 01 2023 00:00:00 GMT+0800 (中国标准时间)
    // date1 Thu Feb 08 2024 00:00:00 GMT+0800 (中国标准时间)
    // Date RegExp对象赋值规则同基本类型一致

    var reg1 = new RegExp("\\w+");
    let reg2 = reg1
    console.log('reg2', reg2)
    reg1 = new RegExp("\\d+")
    console.log('reg1', reg1)
    console.log('reg2', reg2)
    let map1 = new Map();
    // reg2 /\w+/
    // reg1 /\d+/
    // reg2 /\w+/

    map1.set('a', 1);
    map1.set('b', 2);
    map1.set('c', 3);
    let map2 = map1
    console.log('map2', map2)
    map1.set('a', 'aaa')
    console.log('map1', map1)
    console.log('map2', map2)

    // map2 Map(3) {'a' => 1, 'b' => 2, 'c' => 3}
    // map1 Map(3) {'a' => 'aaa', 'b' => 2, 'c' => 3}
    // map2 Map(3) {'a' => 'aaa', 'b' => 2, 'c' => 3}
    // Map Set对象赋值规则同引用类型一致

    let mySet = new Set();

    mySet.add(1); // Set [ 1 ]
    mySet.add(5); // Set [ 1, 5 ]
    mySet.add(5); // Set [ 1, 5 ]
    mySet.add("some text"); // Set [ 1, 5, "some text" ]
    let o = { a: 1, b: 2 };
    mySet.add(o);


    let mySet2 = mySet
    console.log('mySet2', mySet2)
    mySet.delete(5)
    console.log('mySet2', mySet2)
    // mySet2 Set(4) {1, 5, 'some text', {…}}
    // mySet2 Set(3) {1, 'some text', {…}}

```
