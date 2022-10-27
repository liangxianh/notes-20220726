### 1 项目安装webpack webpack-cli后运行webpack命令总是报错

```
command not found: webpack
```
执行了下面内容后可以正常使用了
```
npm install webpack webpack-cli -g
```


### 2 webpack vue.config.js和webpack.config.js区别？

### 3 webpack vue.config.js中configureWebpack和chainWebpack区别？
[参考文档]([https://www.jianshu.com/p/a5fa2629e942](https://www.jianshu.com/p/27d82d98a041))

在vuecli官方文档上有详细的介绍；
```
module.exports = {
  configureWebpack: {
    plugins: [
      new MyAwesomeWebpackPlugin()
    ]
  }
}
```
 configureWebpack 选项提供一个对象：该对象将会被 webpack-merge 合并入最终的 webpack 配置。也可以基于环境去配置
 
 > 警告有些 webpack 选项是基于 vue.config.js 中的值设置的，所以不能直接修改。例如你应该修改 vue.config.js 中的 outputDir 选项而不是修改 output.path；你应该修改 vue.config.js 中的 publicPath 选项而不是修改 output.publicPath。这样做是因为 vue.config.js 中的值会被用在配置里的多个地方，以确保所有的部分都能正常工作在一起。
 
 ```
 // vue.config.js
module.exports = {
  configureWebpack: config => {
    if (process.env.NODE_ENV === 'production') {
      // 为生产环境修改配置...
    } else {
      // 为开发环境修改配置...
    }
  }
}
如下实例：
 configureWebpack: config => {
    if (isProduction) {
      ...
    } else {
      ...
    }
    //直接修改配置
    config.resolve.alias['@asset'] = resolve('src/assets')
  }
  或者
 configureWebpack: config => {
    if (isProduction) {
      ...
    } else {
      ...
    }
    //返回一个将要合并的对象
    return {
      resolve: {
        alias: {
          '@asset':resolve('src/assets')
        }
      }
    } 
  }
 ```
 如果你需要基于环境有条件地配置行为，或者想要直接修改配置，那就换成一个函数 (该函数会在环境变量被设置之后懒执行)。该方法的第一个参数会收到已经解析好的配置。在函数内，你可以直接修改配置，或者返回一个将会被合并的对象：
 ```
// 对比下面的内容chainwebpack内容
 module.exports = {
    configureWebpack: config=>{
        config.plugin=[
            new HappyPack({
                loaders:[
                 {
                    loader: 'babel-loader?cacheDirectory=true',
                }
              ]
            })
        ]
    }
} 

 ```
 
#### 链式操作 (高级)
而Vue CLI 内部的 webpack 配置是通过 webpack-chain 维护的，这个库提供了一个 webpack 原始配置的上层抽象，使其可以定义具名的 loader 规则和具名插件，并有机会在后期进入这些规则并对它们的选项进行修改。
```
// 官方代码示例
config
  .plugin(name)
  .use(WebpackPlugin, args)
```
参数说明

* name 是 webpack-chain 里的key，就是要加入的插件在 webpack-chain 配置里的 key ，就是我们自定义插件的名字,一般我们都保持跟插件名称相同
* WebpackPlugin 使用的 webpack 插件名，在这里，可以直接使用插件，无需进行实例化，就是不需要 new WebpackPlugin()
* args 插件的参数信息。特别注意，args是一个数组，例如 [{},{}] 这种方式，可以配置多个插件实例

```
module.exports = (config) => {
    // set svg-sprite-loader
    config.module
        .rule('svg')
        .uses.clear() // 先删除原有的默认svg rule，写法1,
        // .exclude.add(resolve('src/assets/icons')) // 写法2 针对svg默认规则，忽略src/assets/icons此文件夹下的
        .end()
    config.module
        .rule('icons')
        .test(/\.svg$/)
        .include.add(resolve('./../src/assets/icons'))
        .end()
        .use('svg-sprite-loader')
        .loader('svg-sprite-loader')
        .options({
            symbolId: 'icon-[name]',
        })
        .end()

    //开启happyPack多线程打包
    config.plugin('HappyPack').use(HappyPack, [
        {
            loaders: [
                {
                    loader: 'babel-loader?cacheDirectory=true',
                },
            ],
        },
    ])
}
```

可以看到使用chainWebpack链式写法会简洁很多，不需要new，相当于是一个语法糖吧。

它允许我们更细粒度的控制其内部配置。接下来有一些常见的在 vue.config.js 中的 chainWebpack 修改的例子。
```
// vue.config.js 修改 Loader 选项
module.exports = {
  chainWebpack: config => {
    config.module
      .rule('vue')
      .use('vue-loader')
        .tap(options => {
          // 修改它的选项...
          return options
        })
  }
}
```

### 4 webpack vue.config.js中configureWebpack: (config) => {}和configureWebpack: {}区别？

比如配置生产环境移除console.log利用方法1 可以生效，利用方法二则不生效，但是splitChunks是成功执行了的而且plugins压缩也正常只是optimization不生效；
```
方式1:
  configureWebpack: (config) => {
    config.optimization.minimizer[0].options.minimizer.options.compress.drop_console = true
  },
方式2:
  configureWebpack: {
    // 别名这种应该已经有默认了应该
    resolve: {
      alias: {
        '@': path.join(__dirname, 'src')
      }
    },
    // 也有默认不需要设置了
    devtool: 'source-map',
    plugins: [
      new CompressionPlugin({
        filename: '[path].gz[query]',
        algorithm: 'gzip',
        test: new RegExp('\\.(' + ['js', 'css'].join('|') + ')$'),
        threshold: 10240,
        minRatio: 0.5,
      })
    ],
    optimization: {
      minimizer: [
        new TerserWebpackPlugin({
          terserOptions: {
            compress: {
              warnings: false,
              drop_console: true,
              drop_debugger: true,
              pure_funcs: ['console.log', "console.table"] // 删除console
            }
          },
        }),
      ],
      splitChunks: {
        cacheGroups: {
          libs: {
            name: 'vendor',
            test: /[\\/]node_modules[\\/]/,
            priority: 1,
            chunks: 'initial', // 只打包初始时依赖的第三方
          },

          vant: {
            name: 'vant',
            test: /[\\/]node_modules[\\/]vant[\\/]/,
            chunks: 'all',
            priority: 10,
            reuseExistingChunk: true,
            enforce: true,
          },
        },
      },
    },
  },
```
### 5 移除生产环境的console.log


```
npm install -D babel-plugin-transform-remove-console
之后在babel.config.js中配置
module.exports = {
  presets: [
    '@vue/cli-plugin-babel/preset'
  ],
  env: {
		production: {
			plugins: ["transform-remove-console"]
		},
	},
  // 下面不需要配置，在main.js文件中已引入了
  plugins: [
    [
      'import',
      {
        libraryName: 'vant',
        libraryDirectory: 'es',
        // 指定样式路径
        style: (name) => `${name}/style/less`,
      },
      'vant',
    ],
  ],
}
```
[参考文档](https://juejin.cn/post/6917156163075178509)
 
### 6 针对v-html存在的安全漏洞可以引入xss
```
chainWebpack: config => {
    config.module
      .rule("vue")
      .use("vue-loader")
      .loader("vue-loader")
      .tap(options => {
        options.compilerOptions.directives = {
          html(node, directiveMeta) {
            (node.props || (node.props = [])).push({
              name: "innerHTML",
              value: `xss(_s(${directiveMeta.value}))`
            });
          }
        };
        return options;
      });
  }
  在vue文件中直接使用如下即可：
  <div class="desc-pre" v-html="yourDesc"></div>
  
  或者在mainjs中直接引入
  import xss from 'xss';
  Vue.prototype.$xss = xss
  在vue文件中直接使用如下即可：
  <div class="desc-pre" v-html="$xss(yourDesc)"></div>
```

