# [ webpack4 ](https://webpack.docschina.org/concepts/)

## webpack 安装

- 本地安装webpack
- webpack webpack-cli -D  // 开发依赖

	-- 这里通过yarn安装处理

+ 初始化仓库配置

	```yarn init -y```

	```yarn add webpack webpack-cli -D```

## webpack可以进行0配置
- 打包工具 -> 输出结果(js模块) -> 模块化
- npx 是node模块中的自带运行包的命令 eg：npx webpack
- vscode 安装插件code runner 即可右键执行js文件

## 手动配置webpack
- 默认配置文件名字webpack.config.js
- 如果不是，运行的时候npx webpack --config 名字 (--config 指定别名的意思，配置文件中也是一样的)

	> ```javascript
		// webpack是node写出来的，需要用node语法
		const path = require('path')
		module.exports = {
		  // mode production(压缩，线上，生产环境)，development(非压缩，本地，开发环境过)
		  mode: 'development',
		  entry: './src/index.js',
		  output: {
			filename: 'build.js',
			path: path.resolve(__dirname, 'build')
		  }
		}
	```
- 通过服务地洞并且运行打包之后的代码 webpack-dev-server 插件
	```
	yarn add webpack-dev-server -D
	```
	启动服务  npx webpack-dev-server
- 配置npm运行
	在package.json中
	```js
	 "scripts": {
		"build": "webpack --config webpack.config.js",
		"server": "webpack-dev-server"
	  }
	```
	通过npm run server 就能启动服务了，打包也是一样的,也可用npx webpack打包，一样的

#### 插件CSS

1. 将指定的index.html打包生成到目录中，并且引入相关的js，需要插件

    - ```yarn add html-webpack-plugin -D```

	- plugin 中的配置, putput 中的配置 filename: 'build.[hash:8].js'
	```js
		new HtmlWebpackPlugin({
		  template: './src/index.html',
		  filename: 'index.html',
		  minify: {
			minifyCSS: true, // 压缩css
			collapseWhitespace: true, // 压缩哦html
			minimize: true, // 是否打包为最小值
			removeAttributeQuotes: true, // 去掉引号
			removeComments: true, // 去掉注释
			minifyCSS: true, // 压缩html内的样式
			minifyJS: true, // 压缩html内的js
			removeEmptyElements: true // 清理内容为空的元素标签
		  },
		  hash: true // 避免缓存
		})
	```

	- 解析css模块
			 - 首先配置模块，安装相应的插件，从右向左，自下而上
			 - css -> [style-loader & css-loader] 所有样式文件的基础，必须有的
			 - less -> [less-loader & less]
			 - sass -> [sass-loader & node-sass]
			 - stylus -> [stylus-loader & stylus]
			 - yarn add [上面的插件] -D

		- 然后配置文件进行配置, 例如：
		```js
		 module: {
			rules:[ {test:/\.css$/, use:[ // 从后向前解析，自下而上
			  {
				loader: 'style-loader',
				insertAt: 'top' // 自己写的样式优先
			  },{
				loader: 'css-loader'
			  },
			  'less-loader'
			]}
			]
		  }
		```
2. 将css提取分离出来需要插件

  - ```yarn add mini-css-extract-plugin -D```

  	配置文件配置如下：
		```js
		// webpack是node写出来的，需要用node语法
		const path = require('path')
		const HtmlWebpackPlugin = require('html-webpack-plugin')
		const MiniCssExtractPlugin = require('mini-css-extract-plugin') // 将css文件解析抽离出来的插件
		module.exports = {
		  // mode production(压缩，线上，生产环境)，development(非压缩，本地，开发环境过)
		  mode: 'development',
		  entry: './src/index.js',
		  output: {
			filename: 'build.[hash:8].js',
			path: path.resolve(__dirname, 'build')
		  },
		  module: {
			rules: [
			  {
				test: /\.css$/, use: [
				  MiniCssExtractPlugin.loader, // 将css文件解析抽离成一个文件
				  'css-loader'
				]
			  }
			]
		  },
		  plugins: [
			new HtmlWebpackPlugin({
			  template: './src/index.html',
			  filename: 'index.html'
			}),
			new MiniCssExtractPlugin({
			  filename: '目录名称/main.css' // 起用别名
			})
		  ]
		}
		```

3. 在样式属性前面加上浏览器前缀需要插件

 - ```yarn add postcss-loader autoprefixer -D```
 
		因为样式解析是通过复杂样式 -- css -- style，所以，放在样式loader的最下面，最右边，最后边，引入即可，
		同时，创建前缀配置文件 postcss.config.js, 其配置内容如下：
	```js
		module.exports = {
		  plugins: [require('autoprefixer')]
		}
	```
4. html代码还是开发结构，这里需要将其转换成为生产环境的压缩结构
	插件地址 [压缩插件地址](https://www.npmjs.com/package/mini-css-extract-plugin)

 - ```yarn add optimize-css-assets-webpack-plugin -D```

	- [ ]但是，css压缩了，js还没有，左移还要安装js压缩插件
	```
		yarn add uglifyjs-webpack-plugin -D
	```
		所以配置文件回事这样的：
	```js
		const OptimizeCssAsset = require('optimize-css-assets-webpack-plugin')
		const UglifyJs = require('uglifyjs-webpack-plugin')
		module.exports = {
		  optimization: {
			minimizer: [
			  new OptimizeCssAsset({}),
			  new UglifyJs({
				cache: true, // 缓存
				parallel: true, // 并行处理
				sourceMap: true //调试
			  })
			]
		  },
		  mode: 'production', // 这里注意要改成生产环境，否则是不生效的
		  entry: './src/index.js',
		  output: {
			filename: 'build.[hash:8].js',
			path: path.resolve(__dirname, 'build')
		  }
		}
	```

	- [ ]如果压缩打包的时候产生了错误，看看是不是有es6的语法，有待配置

#### 插件JS

1. js的应用，es6-7的语法越来越普及，也是简单好用，但却不支持浏览器

	- ```yarn add babel-loader @babel/core @babel/preset-env -D```

	- [ ]然后浏览器配置：
	```js
		{
			test: /\.js$/,
			use: [
			  {
				loader: 'babel-loader',
				options: {
				  presets: ['@babel/preset-env'] // 映射
				},
				plugins:[......] // 其他的es6插件
			  }
			]
		  }
	```
2. 全局引用js插件

	- ```yarn add expose-loader -D```

	  在js代码文件中这样引用并使用
	```
		import $ from 'expose-loader?$!jquery' // 将jquery赋值给变量$,然后解析
		console.log(window.$)
	```
	 另一种写法：
	```js
		import $ from 'jquery'
		console.log(window.$)
	```
	 同时配置webpack.config.js
	```js
		{
			test: require.resolve('jquery'),
			use: 'expose-loader?$'
		}
	```
	 第三种方法：

	``` const webpack = require('webpack') ```

	 配置如下：
	```js
		new webpack.ProvidePlugin({
			$: 'jquery'
		})
	```

#### 图片处理

1. 图片属于文件，通过插件打包处理才能生成想要的图片

	- ```yarn add file-loader -D```

	 配置文件的处理
	```js
		{
			test: /\.(png|jpg|gif|jpeg)$/,
			use: 'file-loader',
			publicPath: 'cdn-path'
		}
	```
	 js代码处理如下举例：
	```js
		import kong from './kong.jpg';
		console.log(kong);
		let img = new Image();
		img.src = kong;
		document.body.appendChild(img);
	```

	 如果要在页面中引用图片并且打包，就要安装插件

	- ```yarn add html-withimg-plugin -D```

	 配置文件：
	```js
	  {
        test: /\.html$/,
        use: 'html-withimg-loader'
      }
	```
	 如果图片过大，或者不用http请求，转成base64位的，需要安装插件
	- ```yarn add url-loader -D```

	 配置文件处理：
	```js
	{
		test: /\.(png|jpg|gif|jpeg)$/,
		use: {
		  loader: 'url-loader',
		  options: {
		  	limit: 200*1024,   // 200k
			  outputPath: 'img/' // 生成指定的目录下
		  }
		}
	}
	```

#### 多入口打包处理

1. 直接上代码

	- ```js
		const path = require('path')
		const HtmlWebpackPlugin = require('html-webpack-plugin')

		module.exports = {
		  mode: 'development', // 没有会报警告
		  // 多入口
		  entry: {
			home: './src/index.js',
			other: './src/other.js'
		  },
		  output: {
			filename: '[name].[hash:8].js',
			path: path.resolve(__dirname, 'build')
		  },
		  plugins: [
			// 多入口要对应的写多个
			new HtmlWebpackPlugin({
			  template: './index.html', // 文件来自哪里
			  filename: 'home.html', // 文件生成的名称
			  chunks: ['home'] // 与多入口中的key一样，指定的页面引入指定的js
			}),
			new HtmlWebpackPlugin({
			  template: './index.html',
			  filename: 'other.html',
			  chunks: ['other'] // 根据需求，可以写入多个，数组结构
			})
		  ]
		}
	```

#### source-map --> 调试

+ 配置文件配置即可，不需要安装插件
	- 首先是js文件
	```js
		class A {
			  constructor(){
				console.lo(a)
			  }
		}
		let a = new A();
	```
	- 然后是配置文件
	```js
		const path = require('path')
		const HtmlWebpackPlugin = require('html-webpack-plugin')
		module.exports = {
		  mode: 'production', // 没有会报警告
		  entry: {
			home: './src/index.js'
		  },
		  // 增加映射文件，可以帮助我们调试代码 devtool
		  // source-map 远吗映射，会生成一个独立的sourcemap文件，出错了会标识，定位到行与列，大而全
		  // eval-source-map 不会产生一个单独的sourcemap文件，其他的同source-map一样
		  // cheap-module-source-map 会产生一个单独的sourcemap文件，但是不会产生行与列，定位不到错误位置
		  // cheap-module-eval-source-map 不会产生一个单独的sourcemap文件，会产生在打包后的文件中，
		     定位不到列，能定位到行，也是能找到错误所在行
		  devtool: 'cheap-module-eval-source-map',
		  module: {
			rules: [
			  {
				test: /\.js$/,
				use: {
				  loader: 'babel-loader',
				  options: {
					presets: ['@babel/preset-env']
				  }
				}
			  }
			]
		  },
		  output: {
			filename: 'index.js',
			path: path.resolve(__dirname, 'build')
		  },
		  plugins: [
			new HtmlWebpackPlugin({
			  template: './index.html',
			  filename: 'index.html'
			})
		  ]
		}
	```

>  **操作步骤就是，先npx webpack，然后npx webpack-dev-ser测试**,

#### 实时打包

- 直接上代码

 > ```js
	const path = require('path')
	const HtmlWebpackPlugin = require('html-webpack-plugin')
	module.exports = {
	  mode: 'production', // 没有会报警告
	  entry: {home: './src/index.js'},
	  module: {},
	  watch: true, // 实时打包 - 监控
	  watchOptions: { // 监控的选项
		poll: 1000, // 每秒 1000次
		aggregateTimeout: 500, // 防抖， 我一直在输入
		ignored: /node_modules/ // 忽略项目
	  },
	  output: {filename: 'index.js',path: path.resolve(__dirname, 'build')},
	  plugins: [
		new HtmlWebpackPlugin({template: './index.html',filename: 'index.html'})
	  ]
	}
```


#### webpack 插件

- [x] 清除原有的，打包成为新的，也就是覆盖，清除冗余代码

	 - ` yarn add clean-webpack-plugin -D `

		配置如下：
	```js
		const CleanWebpackPlugin = require('clean-webpack-plugin')
		plugins: [
			new CleanWebpackPlugin(['build']) // 指定要覆盖的目录，数组
	    ]
	```
- [x] 拷贝插件，将原有的文件拷贝到指定的目录

	- `yarn add copy-webpack-plugin -D`

		配置如下：
	```js
		const CopyWebpackPlugin = require('copy-webpack-plugin')
		new CopyWebpackPlugin([
			{from: 'doc', to: './'},
			{...}
		])
	```
- [x] 版权声明，说明这段代码是谁写的

	- `webpack 自带的插件 bannerPlugin`

		配置如下：
	```js
		const webpack = require('webpack')
		new webpack.BannerPlugin('ayil by 2019') // string
	```

#### webpack跨域处理
- 1). 由于webpack-dev-server集成了express框架，所以不用安装，创建一个server.js服务器文件

- ```js
const express = require('express')
let app = express()
app.get('/api/user', (req, res) => {
	res.json({name: 'ayil'})
})
app.listen(3000)
```
- 2). 创建一个访问的客户端文件index.js
- ```js
let xhr = new XMLHttpRequest();
xhr.open('GET', '/api/user', true);
xhr.onload = function () {
    console.log(xhr.response)
};
xhr.send();
```
- 3). 配置文件配置如下：
- ```js
 devServer: {
    proxy: {
      '/api': 'http://localhost:3000' // 目标指向
    }
  },
```
即可访问成功
1. 但是/api是我们自己定义的，而真正的后端过来的接口不是这个样子，那就是我们在处理的时候加上，然后再把它清除就OK了
 - index.js 保持不变，server.js去掉开头的/api即可
```js
devServer: {
    proxy: {
      '/api': {
        target: 'http://localhost:3000',
        pathRewrite: {'/api': ''} // 将api替换掉，转换成为原来的接口形式
      }
    }
  }
```
2. 另一种方法
```js
 before(app){
	app.get('/user', (req, res) => {
	  res.json({ name: 'response' })
	})
  }
```
3. 第三种方法 启动服务端，不用代理，用插件 webpack-middle-software

#### [resolve解析](https://www.webpackjs.com/configuration/resolve/)

#### 定义环境变量
1.  webpack自带的插件

	```javascript
		plugins: [
			new webpack.DefinePlugin({
				DEV: JSON.stringfy('development'), // 默认是变量，而不是字符串，需要转化
				// 这些都是全局变量
			})
		]
	```
2. 区分不同环境

	- `yarn add webpack-merge -D`

		通过这个配置何以将原来的拆分文件进行合并处理
		拆分的文件：
		 	- webpack.conf.dev.js
		 	- webpack.conf.prod.js
		 	- webpack.conf.base.js

	    如果是生产环境就这样配置：
	```javascript
		const base = require('./webpack.conf.base.js')
		const merge = require('webpack-merge')
		module.exports = merge(base, {
			mode: 'production'
		})
	```

 >  在打包时注意： "build": "webpack -- --config ./build/webpack.conf.prod.js"

#### webpack打包时的优化
1. noParse

	在与rules同级中，noParse: /jquery/ // 不去处理jquery的依赖关系

2. ignorePlugin

	在与test统计中,exclude: /node_modules/(排除), include: path.resolve('src')(包含)

3. webpack -- ignorePlugin

	在插件数组中new webpack.IgnorePlugin(/\.\/locale/, /moment/),在什么插件中忽略什么插件
	同时自己import所需要的插件即可
4. 多线程进行打包，加快打包速度（项目大的时候应用，否则慢）

	安装插件happypack ``` yarn add happypack -D```

	配置文件：
	```js
	const HappyPack = require('happypack')
	{
		test: /\.js$/, //  css也是一样的
		use: 'HappyPack/loader?id=js' // 指定下面插件的里面js，联通
	}
	new HappyPack({
		id: 'js',
		use: [{
			loader: 'babel-loader',
			options: {
				presets: ['@babel/preset-env']
			}
		}]
	})
	```
	
#### 多页面打包抽离公共代码

- ```js
module.exports = {
  optimization: {
    splitChunks: { // 分割代码块
      cacheGroups: { //缓存组
        common: {  // 公共的模块
          chunks: 'initial',
          minSize: 0,
          minChunks: 2
        },
        vendor: {
          priority: 1,
          test: /node_modules/,  // 把你抽离出来
          chunks: 'initial',
          minSize: 0,
          minChunks: 2
        }
      }
    }
  }
}```

#### 懒加载

- 懒加载文件，有助于性能的提升
- 安装动态加载插件 `yarn add @babel/plugin-syntax-dynamic-import -D`
- js文件配置：
- 	```js
let btn = document.createElement('button')
btn.innerHTML = 'click'
//vue 的懒加载
btn.addEventListener('click', function () {
  // console.log('click')
  // es6 草案中的语法 jsonp实现动态加载文件
  import('./source.js').then(data => {
    console.log(data.default)
  })
})
document.body.appendChild(btn)
```

#### 热更新

- devServer - hot: true
- ```js
 plugins [
	new webpack.HotModuleReplacementPlugin(), // 热更新插件
	new webpack.NameModulesPlugin() // 哪个模块热更新（打印更新的模块路径）
 ]
 ```
- 页面的配置
- ```js
// 页面实时刷新
import str from './source';
if (module.hot) {
  module.hot.accept('./source', () => {
	let str = require('./source'); // import 只能用在页面的顶端
	console.log(str);
  })
}
```

#### Tapable
> 1. Webpack本质上是一种事件流的机制，他的工作流程就是将各个插件串联起来，实现的核心就是Tapable，核心原理是依赖于发布订阅模式
> 2. [参考地址](https://webpack.js.org/api/plugins/#tapable)