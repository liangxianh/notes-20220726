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
  
  或者在mainjs中直接引入
  import xss from 'xss';
  Vue.prototype.$xss = xss
  在vue文件中直接使用如下即可：
  <div class="desc-pre" v-html="$xss(yourDesc)"></div>
```

