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


1 单纯原型继承弊端：，因实例使用的是同一个原型对象，内存空间是共享的，顾存在耦合性，当构造函数某个属性是复杂对象时，更新其中一个实例的该属性，其它实例也会被修改；
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

2 单纯的构造函数继承，不能继承原型中的属性和方法，但是父类的引用属性不会被共享
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

在上面构造函数继承的继承上加上原型继承
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

