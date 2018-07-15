# Webpack
## 在网页中会引用哪些常用的静态资源？
+ js css image vue


## 网页中静态资源引用多了会有什么问题？？？
1. 网页加载速度慢，因为，我们要发起很多的二次请求
2. 要处理错综复杂的依赖关系


## 如何解决上述问题？
1. 合并、压缩、精灵图、图片的Base64编码
2. 可以使用requirejs，也可以使用webpack

## 什么是webpack？
webpack是前端的一个项目构建工具，它是基于Nodejs开发出来的一个前端工具；

## 如何完美的实现上述两种解决方案
1. 使用gulp
2. 使webpack
+ 借助于webpack这个前端自动化构建工具，可以完美的实现资源合并、打包、压缩、混淆等诸多功能。
+ 根据官网的图片介绍webpack打包过程
+ [webpack官网](https://webpack.js.org/)

## webpack 安装的两种方式
1. 运行`npm install webpack -g` 全局安装webpack，这样就能全局使用webpack的命令
2. 在项目的根目录中`npm install webpack --save-dev`安装到项目的依赖中

## 初步使用 webpack 打包构建列表隔行变色案例
1. 运行 ` npm init ` 初始化项目，使用 npm 管理项目中的依赖包
2. 创建项目的基本目录结构
3. 使用 ` cnpm install jquery --save `安装jQuery类库
4. 创建 `main.js` 并书写隔行变色的代码逻辑
```
	//导入jQuery类库
	import $ from 'jquery'
	//设置偶数行背景色，索引从0开始，0是偶数
	$('#list li:even').css('backgroundColor','lightblue');
	//设置奇数行背景色
	$('#list li:odd').css('backgroundColor','pink');
```
5. 直接在页面上引用 `main.js` 会报错，因为浏览器不认识`import`这种高级js语法，需要使用webpack进行处理，webpack会默认把这种高级的语法转换为低级浏览器识别的语法；
6. 运行 `webpack 入口文件路径 输出文件路径 ` 对 `main.js` 进行处理；
```
	webpack src/js/main.js dist/bundle.js
```
同时修改index页面中的script的src属性的值

## 使用 `html-webpack-plugin` 插件配置启动页面
由于使用`--contentBase`指令的过程比较繁锁，需要指定的启动目录，同时还需要修改index页面中的script的src属性，所以推荐大家使用 `html-webpack-plugin` 插件配置启动页面
1. 运行 `npm i html-webpack-plugin --save-dev `安装到开发依赖
2. 配置 `webpack.config.js` ，配置如下 ： 
```
	//导入处理路径的模块
	const path = require('path');
	//导入自动生成html页面的插件
	const htmlWebpackPlugin = require('html-webpack-plugin')
	//webpack 配置文件
	module.exports = {
		entry: path.resolve(__dirname, './src/main.js'),
		output: {
			path.resolve(__dirname, './dist'),
			filename: 'bundle.js'
		},
		plugins: [  //添加plugins节点配置插件
			new htmlWebpackPlugin({
				tempalte:path.resolve(__dirname, 'src/index.html')，//模板路径
				filename: 'index.html' //自动生成html文件的名称
			})
		]
	}
```
3. 修改`package.json` 中`script`节点中的dev指令如下 ： 
```
	"dev": "webpack-dev-server"
```
4. 将idnex.html中的script标签注释掉，因为 `html-webpack-plugin` 插件会自动把bundle.js注入到html页面中！

## 实现自动打开浏览器，热更新和配置浏览器的默认端口号
**注意：热更新在js中表现的不明显，可以从一会要讲的css身上说明！**
### 方式一：
1. 在package.json中，使用
```
    "scripts": {
		"dev": "webpack-dev-server --open --port 3000 --contentBase src --hot"
  	},
```
+ --open : 表示的是自动打开浏览器
+ --port [端口号] ： 表示的是请求地址的端口号
+ --contentBase src ： 指的是托管项目到src目录下
+ --hot  ： 表示的是热加载，就是自动刷新浏览器

### 方式二 ： 
1. 修改`webpack.config.js`文件，新增`devServer`节点
```
	devSerer:{
		hot: true,
		open: true,
		port: 4312
	}
```
2. 在头部引入`webpackjs`模块；
```
	const webpack = require('webpack');
```
3. 在plugins节点下新增(辅助hot:true,否则功能无效)
```
	new webpack.HotModuleRepalcementPlugin()
```

##使用webpack打包css文件
1. 运行 `npm i style-loader css-loader --save-dev`
2. 修改 `webpack.config.js`文件
```
	module:{
		rules:[ //文件匹配规则
			//处理css文件规则，处理机制是从右向左
			{test:/\.css$/,use:['style-loader','css-loader']}
		]
	}
```

##使用webpack打包less文件
1. 运行 `npm i less-loader less -D`
2. 修改 `webpack.config.js`文件
```
	module:{
		rules:[ //文件匹配规则
			//处理css文件规则，处理机制是从右向左
			{test:/\.less$/,use:['style-loader','css-loader','less-loader']}
		]
	}
```

##使用webpack打包sass文件
1. 运行 `npm i sass-loader node-sass --save-dev`
2. 修改 `webpack.config.js`文件
```
	module:{
		rules:[ //文件匹配规则
			//处理css文件规则，处理机制是从右向左
			{test:/\.scss$/,use:['style-loader','css-loader','scss-loader']}
		]
	}
```
**注意：node-sass可能下载出错，建议用cnpm下载**

##使用webpack打包image文件
1. 运行 `npm i url-loader file-loader --save-dev`
2. 修改 `webpack.config.js`文件
```
	module:{
		rules:[ //文件匹配规则
			//处理css文件规则，处理机制是从右向左
			{test:/\.(jpg|png|gif|bmp|jpeg)$/,use:'url-loader?limit=图片大小（字节）&name=[name].[ext]'}
			limit的值若是大于原始图片的大小，则会生成base64字符串格式，否则是正常的图片格式
			【name】表示图片原来是什么名字，修改之后还是什么名字
			【ext】是固定格式
			多个图片 相同名称  会覆盖   &name=[hash:8]-[name].[ext]会处理 8 表示截取hash值的前八位
		]
	}
```
##使用webpack打包字体文件
1. 运行 `npm i url-loader file-loader --save-dev`
2. 修改 `webpack.config.js`文件
```
	module:{
		rules:[ //文件匹配规则
			//处理css文件规则，处理机制是从右向左,url-loader也能处理字体图标文件
			{test:/\.(ttf|eot|svg|woff|woff2)$/,use:'url-loader'}
		]
	}
```

**注意： render的用法跟 【组件】 的区别是： v-text与差值表达式的区别，全部替换与替换一部分，这里，render是一个函数，通过return 参数(createElements)（组件名称）**

## 使用webpack处理es6
在webpack中，默认只能处理一部分es6的新语法，一些更高级的es6语法，或者es7语法，webpack是处理不了的，这时候，就需要借助第三方的loader，来帮助处理这些更更高级的语法，babel

1. 在webpack中，可以运行如下两套命令，安装两套包，去安装babel的相关loader
	1.1 第一套包：babel-core babel-loader babel-plugin-transform-runtime
	1.2 第二套包：babel-preset-env babel-preset-stage-0
2. 打开webpack配置文件，在module的节点下的rules中，添加一个新的匹配规则：
	2.1 {test:/\/js$/,use:'babel-loader',exclude:/node_modules/}
	2.2 注意：在babel的loader规则中，必须把node_modules目录通过exclude选项排除掉，原因有两个：
		2.2.1 如果不排除掉，babel会把node_module中所有的第三方js文件都打包编译，这样会非常消耗CPU，同时，打包的速度也是非常的慢
		2.2.2 哪怕babel把所有的node_modules中的js文件转换完了，但是项目也是无法正常运行的
3. 在项目的根目录中，新建一个叫做.babelrc的babel配置文件，这个配置文件属于JSON格式，所以，在写babelrc配置的时候，必须符合JSON语法规范，不能写注释，字符串必须用双引号
	3.1 在 .babelrc 写如下的配置，可以把preset翻译成 【语法】的意思
	```
		{
			"preset": ["env", "stage-0"],
			"plugins": ["transform-runtime"]
		}
	```
4. babel-preset-env 是最新的es6语法，功能也是相对来说最完善的 


## git 提交代码
	   git checkout 分之名称 -------------  切换分支
	   
	   git 账号密码 1239423535@qq.com   zx32zx32

### vue时间过滤器    {{ time | dataFormat }} 安装moment插件
```
	Vue.filter('dataFormat',function('timeStr',pattern="yyyy-MM-dd HH:mm:ss"){
		return moment(timeStr).format(pattern);
	})
```

###### 在点击加载更多的时候,利用页面值++,再调用请求数据的方法s
**做加载更多的时候,注意做拼接操作,用concat数组拼接**

##### git 配置公钥
+ git config --global user.name "liya"
+ git config --global user.email "1239423535@qq.com"
+ ssh-keygen -t rsa -C "1239423535@qq.com"

##### promise
```
	promise是为了解决回调地狱的机制,代码不一定会少 大写说明他是一个构造函数  而且是异步的
	因为是异步操作,所以不能return返回,这时候需要通过回调函数进行处理 new出来之后会立即执行
	function是按需执行,其他的是立即执行如果有错误处理的时候,再多个回调也不会影响
```

### 将项目映射到外网   通过ngrok就可以了,修改端口号或者不修改
















