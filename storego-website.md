### 官网开发遇到的问题

> 设计方案： 纯静态页面，利用webpack进行打包和压缩，因为要适应移动端和web端；可以下适应移动端（主流的），在利用媒体文件查询写web端样式；对于布局的不同可以看css是否可以实现
比如：移动端排版不同，可以利用，flex-direction: row-reverse;进行左右位置互换，但是也是需要单独的设置样式；若位置不同可以利用dom的display：none；来实现等；

下面是全部的webpack相关的配置文件；
```
webpack.config.js内容

const path = require("path");
const IP = require('ip')
const ESLintWebpackPlugin = require("eslint-webpack-plugin");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const CompressionPlugin = require("compression-webpack-plugin");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const CssMinimizerPlugin = require("css-minimizer-webpack-plugin");

const { resolve } = require("path");

module.exports = {
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "dist"), // 生产模式需要输出
    filename: "js/[name].[hash:8].js", // 将 js 文件输出到 static/js 目录中
    clean: true,
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [MiniCssExtractPlugin.loader, "css-loader", "postcss-loader"],
      },
      {
        test: /\.(png|jpe?g|gif|webp)$/,
        type: "asset",
        parser: {
          dataUrlCondition: {
            maxSize: 10 * 1024, // 小于10kb的图片会被base64处理
          },
        },
        generator: {
          filename: "imgs/[hash:8].[name][ext]",
        },
      },
      {
        test: /\.(html)$/,
        // 需要安装html-loader 命令: npm i html-loader -D
        loader: 'html-loader'
      },
      {
        test: /\.js$/,
        exclude: /node_modules/, // 排除node_modules代码不编译
        loader: "babel-loader",
        options: {
          presets: ['@babel/preset-env']
        }
      },
    ],
  },
  plugins: [
    new ESLintWebpackPlugin({
      // 指定检查文件的根目录
      context: path.resolve(__dirname, "../src"),
    }),
    new HtmlWebpackPlugin({
      // 以 public/index.html 为模板创建文件
      // 新的html文件有两个特点：1. 内容和源文件一致 2. 自动引入打包生成的js等资源
      template: path.resolve(__dirname, "./src/index.html"),
      minify: {
        collapseWhitespace: true,
        removeComments: true
      },
      favicon: resolve(__dirname, 'src/favicon.ico')
    }),
    new CompressionPlugin({
      // filename: '[path].gz[query]',
      filename: '[path][name][ext].gz[query]',
      algorithm: 'gzip',
      test: new RegExp('\\.(' + ['js', 'css'].join('|') + ')$'),
      threshold: 10240,
      minRatio: 0.5,
    }),
    // 提取css成单独文件
    new MiniCssExtractPlugin({
      // 定义输出文件名和目录
      filename: "css/main.[hash:8].css",
    }),
    new CssMinimizerPlugin(),
  ],
  devServer: {
    hot: true,
    // host: "IP.address()", // 启动服务器域名
    port: "3000", // 启动服务器端口号
    open: true, // 是否自动打开浏览器
    https: false,
    proxy: {
      '/amatooke': {
        target: 'https://test.amatooke.com',
        secure: false,
        changeOrigin: true,
      },
      // '/amatooke': 'https://test.amatooke.com/',
    },
  },
  mode: "production",
};
```

还需要对postcss中使用插件将px转换为vw等
在postcss.config.js中配置
```
module.exports = {
  plugins: {
    'postcss-px-to-viewport': {
      unitToConvert: 'px',    // 需要转换的单位，默认为"px"
      viewportWidth: 360,     // 设计稿的视窗宽度
      propList: ['*', '!font-size'], // 能转化为 vw 的属性列表
      exclude: undefined,     // 忽略某些文件夹下的文件或特定文件，例如 'node_modules' 下的文件
      include: /\/src\//,     // 如果设置了include，那将只有匹配到的文件才会被转换
      landscapeWidth: 640,   // 横屏时使用的视窗宽度
      // mediaQuery: true,  //设置媒体查询里的单位是否需要转换单位
    }
  }
}
```
相关依赖包版本
```
"devDependencies": {
    "@babel/core": "^7.19.6",
    "@babel/preset-env": "^7.19.4",
    "babel-loader": "^8.2.5",
    "compression-webpack-plugin": "^10.0.0",
    "css-loader": "^6.7.1",
    "css-minimizer-webpack-plugin": "^4.2.2",
    "eslint": "^8.26.0",
    "eslint-webpack-plugin": "^3.2.0",
    "html-loader": "^4.2.0",
    "html-webpack-plugin": "^5.5.0",
    "ip": "^1.1.8",
    "less-loader": "^11.1.0",
    "mini-css-extract-plugin": "^2.6.1",
    "postcss-loader": "^7.0.1",
    "postcss-px-to-viewport": "^1.1.1",
    "style-loader": "^3.3.1",
    "webpack": "^5.74.0",
    "webpack-cli": "^4.10.0",
    "webpack-dev-server": "^4.11.1"
  }
```

#### 1 webpack如何利用babel转译js代码
如上：
```
     {
        test: /\.js$/,
        exclude: /node_modules/, // 排除node_modules代码不编译
        loader: "babel-loader",
        options: {
          presets: ['@babel/preset-env']
        }
      },
```
[多页面参考]（https://github.com/vhtml/webpack-MultiPage-static/blob/master/webpack.config.js）

#### 2 webpack如何处理图片资源；
```
module.exports = {
    module: {
       rules: [
              {
                test: /\.(png|jpe?g|gif|webp)$/,
                type: "asset",
                parser: {
                  dataUrlCondition: {
                    maxSize: 10 * 1024, // 小于10kb的图片会被base64处理
                  },
                },
                generator: {
                  filename: "imgs/[hash:8].[name][ext]",
                },
              },
              {
                test: /\.(html)$/,
                // 需要安装html-loader 命令: npm i html-loader -D
                loader: 'html-loader'
              },
       ]
    }
}
```

#### 3 webpack如何处理ico资源

```
    new HtmlWebpackPlugin({
      // 以 public/index.html 为模板创建文件
      // 新的html文件有两个特点：1. 内容和源文件一致 2. 自动引入打包生成的js等资源
      template: path.resolve(__dirname, "./src/index.html"),
      minify: {
        collapseWhitespace: true,
        removeComments: true
      },
      favicon: resolve(__dirname, 'src/favicon.ico')
    }),
```

#### 4 webpack纯静态资源如何对原生js实现的内容接口请求进行接口转发
```
devServer: {
    hot: true,
    // host: "IP.address()", // 启动服务器域名
    port: "3000", // 启动服务器端口号
    open: true, // 是否自动打开浏览器
    https: false,
    proxy: {
      '/***': {
        target: 'https://test.***.com',
        secure: false,
        changeOrigin: true,
      },
    },
  },
```
对本地进行接口转发时需要注意：项目里面接口请求若书写的是全路径的地址，就不会走本地的转发；
即当接口请求时直接使用了原路径：'https://test.*.com/publicpath/v1/file/downloadApp  则在devserver里面配置的接口转发将会不弃作用；

#### 5 webapck开发环境启动后发现devserver设置的接口转发无效，或者说启动没有走这里
实际是走了的，只是接口写了全路径没有走到本地；需要设置类似这种的请求路径 在env里面进行设置环境变量，配置window.BASE_URL = "https://api.amatooke.com/";
apiUrl = '/public/v1/file/downloadApp'

#### 6 更新代码后为进行编辑处理直接push到远程，部署到服务器后发现打包后的代码并没有更新；
构建打包错项目了

#### 7 原生js实现post请求，Content-Type设置不同时数据解析需要不同的内容

```
xhr.setRequestHeader("Content-Type", "application/json"); 时：
请求参数需要处理一下：
const data = JSON.stringify({'versionCode': 1})
返回的内容也需要处理一下：
res = JSON.parse(res)

xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded")时，
请求参数需要如下格式；
const data = 'versionCode=1'

```

#### 8 纯原生页面如何处理html先加载而js和css后加载导致的页面混乱问题
正常情况下css和js同在一般会先加载css 在加载js，而webpack打包将样式和js内容都打包到了js文件内，造成比较大，加载很慢就会出现html内容先展示，无任何样式，就显得有点乱码一样；
顾只要将css抽取出来即可了使用mini-css-extract-plugin插件
```
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
 module: {
    rules: [
      {
        test: /\.css$/,
        // use: ["style-loader", "css-loader", "postcss-loader"],
        // 将style-loader替换为miniloader
        // MiniCssExtractPlugin.loader 抽取css到独立的文件 并引入到index.html的header中（注意下面的提示可能需要HtmlWebpackPlugin配合） <link href="css/main.dbdebb86.css" rel="stylesheet">
        // style-loader --- Inject CSS into the DOM. 动态创建style标签(在output的js文件中)
        use: [MiniCssExtractPlugin.loader, "css-loader", "postcss-loader"],
      },
    ]
 }
```
这样打包出来的内容是没有进行压缩的，需要压缩需要使用插件css-minimizer-webpack-plugin
```
const CssMinimizerPlugin = require("css-minimizer-webpack-plugin");
plugins: [
    ...
    // 提取css成单独文件
    new MiniCssExtractPlugin({
      // 定义输出文件名和目录
      filename: "css/main.[hash:8].css",
    }),
    // 压缩css文件
    new CssMinimizerPlugin(),
  ],
```
> Note that if you import CSS from your webpack entrypoint or import styles in the initial chunk, mini-css-extract-plugin will not load this CSS into the page. Please use html-webpack-plugin for automatic generation link tags or create index.html file with link tag.

#### 9 利用webpack压缩打包出来的gz资源，服务器怎么开启加载gz资源还是原资源呢？

在本地nginx中配置gzip on 发现不起作用；因为webpack压缩好的内容对nginx来说属于静态压缩，使用的是现成的扩展名为.gz的预压缩文件；如下即可
```
gzip on;
gzip_static on;
```

```
压缩开启之前 DOMContentLoaded: 1.47 s Load: 1.64 s | 2.35s
压缩开启之后 DOMContentLoaded: 758 ms Load: 866 ms

vendor.js 401k----118k
vant.js 155k -----46k
app.js  15k ----- 5k
...
vant.css 190k ----- 72k
详细对比如下：
```
![rRoF5w8Mu5](https://user-images.githubusercontent.com/31762176/200975597-ed630df5-dabd-4ec8-a8ea-92b110c13d26.jpg)
![36sLfoeFKS](https://user-images.githubusercontent.com/31762176/200975617-e03b08c9-579a-4958-a0f5-371ba3cff649.jpg)

app-h5商城项目利用vant vue 性能提升有一半的样子；

#### 10 nginx进行压缩资源和webpack压缩

理解是nginx自身可以对静态资源进行gz，若设置了压缩即使自己webpack不压缩也会被处理；web端一般利用webpack直接gz好，远程nginx直接gzip on；即可
关于 nginx 的 gzip ，可以分为两种：

nginx 动态压缩，对每个请求先压缩再输出。
```
gzip on;
    # gzip_comp_level 5;
    # gzip_min_length 1k;
    # gzip_buffers 4 16k;
    # gzip_proxied any;
    # gzip_vary on;
    # gzip_types
    #   application/javascript
    #   application/x-javascript
    #   text/javascript
    #   text/css
    #   text/xml
    #   application/xhtml+xml
    #   application/xml
    #   application/atom+xml
    #   application/rdf+xml
    #   application/rss+xml
    #   application/geo+json
    #   application/json
    #   application/ld+json
    #   application/manifest+json
    #   application/x-web-app-manifest+json
    #   image/svg+xml
    #   text/x-cross-domain-policy;
    gzip_static on;  
    # gzip_disable "MSIE [1-6]\.";  
```
nginx 静态压缩，使用现成的扩展名为 .gz 的预压缩文件。
```
gzip on;
gzip_static on;
```

不同之处在于：
* Webpack压缩会在构建运行期间一次压缩文件，然后将这些压缩版本保存到磁盘。
* nginx在请求时压缩文件时，某些包可能内置了缓存,因此性能损失只发生一次(或不经常),但通常不同之处在于,这将在响应 HTTP请求时发生。
* 对于实时压缩,让上游代理(例如 Nginx)处理gzip和<b>缓存通常更高效!!!!!!!!!!!!!</b>，因为它们是专门为此而构建的,并且不会遭受服务端程序运行时的开销(许多都是用C语言编写的) 。
* 使用 Webpack的好处是， Nginx每次请求服务端都要压缩很久才回返回信息回来，不仅服务器开销会增大很多，请求方也会等的不耐烦。我们在 Webpack打包时就直接生成高压缩等级的文件，作为静态资源放在服务器上，这时将 Nginx作为二重保障就会高效很多。

注：具体是在请求时实时压缩，或在构建时去生成压缩文件，就要看项目业务情况。


[nginx之动态压缩和静态压缩参考文](https://blog.csdn.net/fxss5201/article/details/106535475)

[开启双端gzip及优缺点](https://juejin.cn/post/6844903825585897485)

[mac环境安装nginx](https://www.cnblogs.com/tugenhua0707/p/9863885.html)


