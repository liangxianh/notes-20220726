### js

#### 1 数据类型

* 基本数据类型 Number String Boolean Undefined null symbol
* 引用数据类型 object array function 
* 其他引用数据类型 Date Regexp map set等 

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

#### 2 js 继承

* 原型继承
* 构造函数继承
* 组合
* 原型式继承
* 寄生式继承
* 寄生组合式继承


> 1 单纯原型继承弊端：，因实例使用的是同一个原型对象，内存空间是共享的，顾存在耦合性，当构造函数某个属性是复杂对象时，更新其中一个实例的该属性，其它实例也会被修改；

```
    function Parent() {
      this.name = 'parent1';
      this.play = [1, 2, 3]
    }
    function Child1() {
      this.type = 'child1';
    }
    Child1.prototype = new Parent();
    console.log(new Child1())
    var s1 = new Child1();
    var s2 = new Child1();
    s1.play.push(4);
    console.log(s1, s2); // [1,2,3,4]
```

> 2 单纯的构造函数继承，不能继承原型中的属性和方法，但是父类的引用属性不会被共享

```
function Person(name, age) {
      this.name = name
      this.age = age
      this.eat = function () {
        console.log('eat')
      }
      this.hoppy = [
        'sleep', 'eat', 'play'
      ]
    }
    Person.prototype.say = function () {
      console.log(this.name, 'hello')
    }
    Person.prototype.height = '160cm'

    function Student(name, age, grade) {
      // 进行构造函数继承
      Person.call(this, name, age)
      this.grade = grade
    }
    let stu1 = new Student('louella', 18, 100)
    let stu11 = new Student('louella111', 22, 66)
    console.log(stu1)
    console.log(stu1.hoppy)
    stu1.hoppy.push('watch tv')
    // stu1.say() //Uncaught TypeError: stu1.say is not a function
    stu1.eat()
    console.log('stu1,stu11--------', stu1, stu11)
```

> 3 组合继承 在上面构造函数继承的继承上加上原型继承

* 重点：结合了两种模式的优点，传参和复用
* 特点：1、可以继承父类原型上的属性，可以传参，可复用。
     2、每个新实例引入的构造函数属性是私有的。
* 缺点：调用了两次父类构造函数（耗内存），子类的构造函数会代替原型上的那个父类构造函数。

```
    // 2 原型继承，用于继承原型上的属性和方法；
    // Student.prototype = Person.prototype // 这种方式耦合性太强，子和父之间共享空间
    Student.prototype = new Person()
    // 上面这种new会将name和age等undefined值也复制过来（不影响，会先从对象本身寻找变量名），但是原型上的say和height也能很好的继承过来，且相互不影响；
    // 新增其他方法
    Student.prototype.printGrade = function () {
      console.log(this.name, this.grade)
    }
    // 覆盖之前方法，或者在之前方法新增内容
    // Student.prototype.say = function () {
    //   console.log(this.name, this.grade)
    // }
    // 增强
    Student.prototype.say2 = function () {
      this.say()
      console.log(this.name, this.grade)
    }
    let stu2 = new Student('i am stu2', 19, 99)
    console.log(stu2)
    stu2.say()
    stu2.say2()
```
但是组合继承 父类执行两次，造成对构造一次的这个开销
```
function Parent3 () {
    this.name = 'parent3';
    this.play = [1, 2, 3];
}

Parent3.prototype.getName = function () {
    return this.name;
}
function Child3() {
    // 第二次调用 Parent3()
    Parent3.call(this);
    this.type = 'child3';
}

// 第一次调用 Parent3()
Child3.prototype = new Parent3();
// 手动挂上构造器，指向自己的构造函数
Child3.prototype.constructor = Child3;
var s3 = new Child3();
var s4 = new Child3();
s3.play.push(4);
console.log(s3.play, s4.play);  // 不互相影响
console.log(s3.getName()); // 正常输出'parent3'
console.log(s4.getName()); // 正常输出'parent3'
```


> 4 原型式继承（object.create()）

    * 重点：用一个函数包装一个对象，然后返回这个函数的调用，这个函数就变成了个可以随意增添属性的实例或对象。object.create()就是这个原理。
    * 特点：类似于复制一个对象，用函数来包装。
    * 缺点：1、所有实例都会继承原型上的属性。
         2、无法实现复用。（新实例属性都是后面添加的）
      （ Object.create方法实现的是浅拷贝）
```
    function createObj(o) {
      function F() { }
      F.prototype = o
      return new F()
    }
    let person = {
      name: 'llm',
      age: 23,
      obj: {
        asd: 123
      },
      say: function () {
        console.log('AAAA');
      }
    }
    let person1 = createObj(person)
    let person2 = createObj(person)
    console.log(person1.say())
    person1.obj.asd = 987
    // person1, person2的obj一样，因为F.prototype=o是浅复制
    console.log(person1.obj);
    console.log(person2.obj);
```

> 5 寄生继承

    * 重点：就是给原型式继承外面套了个壳子。
    * 优点：没有创建自定义类型，因为只是套了个壳子返回对象（这个），这个函数顺理成章就成了创建的新对象。
    * 缺点：没用到原型，无法复用(浅拷贝)。
```
    function createObj(o) {
      let clone = Object.create(o)
      return clone
    }
    let person = {
      name: 'llm',
      age: 23,
      say: function () {
        console.log('AAA');
      }
    }
    let person1 = createObj(person)
    let person2 = createObj(person)
    console.log(person1.say());
    person2.age = 24
    console.log(person2.age);
    console.log(person1.age);
```

> 6 寄生组合继承（常用）

* 重点：修复了组合继承的问题
```
function clone (parent, child) {
    // 这里改用 Object.create 就可以减少组合继承中多进行一次构造的过程
    child.prototype = Object.create(parent.prototype);
    child.prototype.constructor = child;
}

function Parent6() {
    this.name = 'parent6';
    this.play = [1, 2, 3];
}
Parent6.prototype.getName = function () {
    return this.name;
}
function Child6() {
    Parent6.call(this);
    this.friends = 'child5';
}
/* 
  与组合继承不同的是将Student.prototype = new Person()换成了下面的方式
*/
clone(Parent6, Child6);

Child6.prototype.getFriends = function () {
    return this.friends;
}

let person6 = new Child6();
console.log(person6); //{friends:"child5",name:"child5",play:[1,2,3],__proto__:Parent6}
console.log(person6.getName()); // parent6
console.log(person6.getFriends()); // child5
```

 > 7 使用ES6 中的extends关键字直接实现 JavaScript的继承

```
class Person {
  constructor(name) {
    this.name = name
  }
  // 原型方法
  // 即 Person.prototype.getName = function() { }
  // 下面可以简写为 getName() {...}
  getName = function () {
    console.log('Person:', this.name)
  }
}
class Gamer extends Person {
  constructor(name, age) {
    // 子类中存在构造函数，则需要在使用“this”之前首先调用 super()。
    super(name)
    this.age = age
  }
}
const asuna = new Gamer('Asuna', 20)
asuna.getName() // 成功访问到父类的方法
```
利用babel工具进行转换，我们会发现extends实际采用的也是寄生组合继承方式，因此也证明了这种方式是较优的解决继承的方式


详细对比6和7 发现6还存在如下问题:不能解决原型属性为引用类型时 浅拷贝问题；
```
   function clone(parent, child) {
      // 这里改用 Object.create 就可以减少组合继承中多进行一次构造的过程
      child.prototype = Object.create(parent.prototype);
      child.prototype.constructor = child;
    }

    function Parent6() {
      this.name = 'parent6';
      this.play = [1, 2, 3];
    }
    Parent6.prototype.hoppy = ['sleep', 'eat']
    function Child6() {
      Parent6.call(this);
      this.friends = 'child5';
    }
    clone(Parent6, Child6);
    
    console.log('---------------寄生组合式继承')
    let person6 = new Child6();
    let person7 = new Child6();
    person6.play.push(7)
    console.log('person6', person6);  // Child6 {name: 'parent6', play: Array(4), friends: 'child5'}
    console.log('person7', person7);  // Child6 {name: 'parent6', play: Array(3), friends: 'child5'}
    // 发现寄生式组合继承，不能解决原型属性为引用类型时 浅拷贝问题；
    console.log('person6.hoppy', person6.hoppy)
    person6.hoppy.push('play')
    console.log('person6.hoppy', person6.hoppy) // ['sleep', 'eat', 'play']
    console.log('person7.hoppy', person7.hoppy) // ['sleep', 'eat', 'play']
```

但是extends解决了这个问题
```
    class Persone {
      constructor(name) {
        this.name = name
      }
      // 原型方法
      // 即 Person.prototype.getName = function() { }
      // 下面可以简写为 getName() {...}
      getName = function () {
        console.log('Person:', this.name)
      }
      hoppy = ['eat', 'sleep']
    }
    class Gamer extends Persone {
      constructor(name, age) {
        // 子类中存在构造函数，则需要在使用“this”之前首先调用 super()。
        super(name)
        this.age = age
      }
    }
    const asuna = new Gamer('Asuna', 20)
    const lili = new Gamer('lili', 30)
    // asuna.getName() // 成功访问到父类的方法
    asuna.hoppy.push('kkk')
    console.log(asuna.hoppy) // ['eat', 'sleep', 'kkk']
    console.log(lili.hoppy) // ['eat', 'sleep']
```
单纯的看object.create用于创建一个新对象，使用现有的对象来作为新创建对象的原型（prototype）
```
    const myVars = {
      key1: 'sample',
      key2: ['s', 'a'],
      fun: function () {
        console.log('fun')
      }
    }
    let sam1 = Object.create(myVars)
    let sam2 = Object.create(myVars)
    sam1.key2.push('111')
    console.log('', sam1, sam2)
```
明显的会共用内存空间（浅拷贝）
![image](https://user-images.githubusercontent.com/31762176/219313445-ea894ea5-2f8b-4efc-b9c9-8bda14767895.png)

 > 8 js事件循环

* 同步:正常的js
* 异步:ajax等

* 宏任务：正常的js setTimeout/setInterval UIrendering/UI事件 postMessage messageChannel setImmediate I/O（node）
* 微任务：promise.then MutaionObserver Object.observe process.nextTick

执行机制
* 同步---异步
* 宏任务（一般是同步的）-->微任务（一般是异步的，队列里的所有微任务）-->下一个宏任务（一般是异步的）

**注意：**
1. promise里正常同步执行只是then函数会加入到异步微任务里；
2. async await中await会阻塞后面的；await fn1；fn2； fn1会正常执行，fn2会被阻塞加入到微任务队列里；

详细内容

* MutaionObserver
```
   <div id="observer-id">
    <input type="text">
   </div>
  
   // 选择需要观察变动的节点
    const targetNode = document.getElementById('observer-id');

    // 观察器的配置（需要观察什么变动）
    const config = { attributes: true, childList: true, subtree: true };

    // 当观察到变动时执行的回调函数
    const callback = function (mutationsList, observer) {
      // Use traditional 'for loops' for IE 11
      for (let mutation of mutationsList) {
        if (mutation.type === 'childList') {
          console.log('A child node has been added or removed.');
        }
        else if (mutation.type === 'attributes') {
          console.log('The ' + mutation.attributeName + ' attribute was modified.');
        }
      }
    };

    // 创建一个观察器实例并传入回调函数
    const observer = new MutationObserver(callback);

    // 以上述配置开始观察目标节点
    console.log(111)
    observer.observe(targetNode, config);

    // 之后，可停止观察
    // observer.disconnect();
    var elea = document.createElement('a')
    elea.href = "https://www.baidu.com"
    targetNode.appendChild(elea)

    new Promise(function (resolve) {
      console.log('promise')
      resolve()
    }).then(function () {
      console.log('promise.then')
    })
    setTimeout(() => {
      console.log('timeout')
    }, 1)
    console.log(222)

    /*
       111
       promise
       222
       A child node has been added or removed.
       promise.then
       timeout
     */
```
[MutationObserver参考地址](https://developer.mozilla.org/zh-CN/docs/Web/API/MutationObserver)

object.observe(已废弃，用proxy对象替代)
```
    // proxy 同步,非异步的宏任务；
    const handler = {
      get: function (obj, prop) {
        console.log('proxy-handler')
        return prop in obj ? obj[prop] : 37;
      }
    };

    const p = new Proxy({}, handler);
    p.a = 1;
    p.b = undefined;

    console.log(p.a, p.b);      // 1, undefined
    console.log('c' in p, p.c); // false, 37
```

> 9 Bom（Browser object model）

[参考文档](https://vue3js.cn/interview/JavaScript/BOM.html#%E4%BA%8C%E3%80%81window)
* window

```
<button onclick="openWin()">Open "myWindow"</button>
<button onclick="moveWin()">Move "myWindow"</button>

<script>
let myWindow;

function openWin() {
  myWindow = window.open("", "", "width=400, height=400");
}

function moveWin() {
  myWindow.moveBy(250, 250);
}
</script>
```

* location
* navigator
* screen
* history

> 10 尾递归
```
// 普通
function factorial(n) {
  if (n === 1) return 1;
  return n * factorial(n - 1);
}

factorial(5) // 120

// 尾递归
function factorial(n, total) {
  if (n === 1) return total;
  return factorial(n - 1, n * total);
}

factorial(5, 1) // 120
```
如果n等于5，这个方法要执行5次，才返回最终的计算表达式，这样每次都要保存这个方法，就容易造成栈溢出，复杂度为O(n)

如果我们使用尾递归，可以看到，每一次返回的就是一个新的函数，不带上一个函数的参数，也就不需要储存上一个函数了。尾递归只需要保存一个调用栈，复杂度 O(1)
尾递归应用场景：
```
数组求和

function sumArray(arr, total) {
    if(arr.length === 1) {
        return total
    }
    return sum(arr, total + arr.pop())
}
使用尾递归优化求斐波那契数列

function factorial2 (n, start = 1, total = 1) {
    if(n <= 2){
        return total
    }
    return factorial2 (n -1, total, total + start)
}
数组扁平化

let a = [1,2,3, [1,2,3, [1,2,3]]]
// 变成
let a = [1,2,3,1,2,3,1,2,3]
// 具体实现
function flat(arr = [], result = []) {
    arr.forEach(v => {
        if(Array.isArray(v)) {
            result = result.concat(flat(v, []))
        }else {
            result.push(v)
        }
    })
    return result
}
数组对象格式化

let obj = {
    a: '1',
    b: {
        c: '2',
        D: {
            E: '3'
        }
    }
}
// 转化为如下：
let obj = {
    a: '1',
    b: {
        c: '2',
        d: {
            e: '3'
        }
    }
}

// 代码实现
function keysLower(obj) {
    let reg = new RegExp("([A-Z]+)", "g");
    for (let key in obj) {
        if (obj.hasOwnProperty(key)) {
            let temp = obj[key];
            if (reg.test(key.toString())) {
                // 将修改后的属性名重新赋值给temp，并在对象obj内添加一个转换后的属性
                temp = obj[key.replace(reg, function (result) {
                    return result.toLowerCase()
                })] = obj[key];
                // 将之前大写的键属性删除
                delete obj[key];
            }
            // 如果属性是对象或者数组，重新执行函数
            if (typeof temp === 'object' || Object.prototype.toString.call(temp) === '[object Array]') {
                keysLower(temp);
            }
        }
    }
    return obj;
};
```
