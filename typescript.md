### 安装与编译
> 1 安装

```
npm i -g typescript
```

> 2 编写ts文件，然后编译为js内容

```
tsc --init  //会生成一个tsconfig.json文件
tsc    // 就会将目录里的ts文件编译为同名的js文件
tsc -w  // watch 每当文件保存时就会自动编译文件
```

### 语法

核心，定义任何东西的时候要注明类型，调用任何东西的时候要检察类型！！！
> 3 类型

```
// 类型需要小写，
let type: string = 'string'

// 断言 明确告诉这个按钮确实存在；
let button = document.querySelector('button') as HTMLButtonElement

// 联合类型，button有可能为空
let button: HTMLButtonElement | null = document.querySelector('button')

```
> 类，汇集和服用数据

```
class Cat {
  // 会有类型下拉线提示；
  constructor(id, url, height, width) {
    this.id = id;
    this.url = url;
    this.height = height;
    this.width = width;
  }
}
```

> 接口，为对象进行类型的定义；创建对象时多个对象就可以复用这个接口

```

```

> 接口

```

```

> 接口

```

```
