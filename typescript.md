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

> 3 类型

```
// 类型需要小写，
let type: string = 'string'

// 断言 明确告诉这个按钮确实存在；
let button = document.querySelector('button') as HTMLButtonElement

// 联合类型，button有可能为空
let button: HTMLButtonElement | null = document.querySelector('button')

```
