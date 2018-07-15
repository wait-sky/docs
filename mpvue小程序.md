# 小程序
###### 我的appid {  wx8ca01bfe6b9de653  }

##  小程序框架 [mpvue官网](http://mpvue.com/mpvue/)

##### 全局安装 vue-cli
     npm install --global vue-cli

##### 创建一个基于 mpvue-quickstart 模板的新项目
     vue init mpvue/mpvue-quickstart my-project

##### 安装依赖
     cd my-project
     npm install
##### 启动构建
     npm run dev
                              ========================前端介绍说明与配置==============================
#	mpvue  小程序的框架

##	mpvue是什么？
+ mpvue一个基于Vue的微信小程序前端框架，可以让我们用vue的语法写小程序的项目。
+ mpvue 是一个使用 Vue.js 开发小程序的前端框架。框架基于 Vue.js 核心，mpvue 修改了 Vue.js 的 `runtime` 和 `compiler` 实现，使其可以运行在小程序环境中，从而为小程序开发引入了整套 Vue.js 开发体验。

## mpvue官网
[mpvue 官网](http://mpvue.com/)

[mpvue 详细API](http://mpvue.com/mpvue/#_2)

## mpvue是哪个公司的？
`美团旗下产品：美团汽车票 和 美团充电使用，此外，正有一大批小程序正在接入中。`

## mpvue未来的目标？
	在未来最理想的状态是，可以一套代码可以直接跑在多端：WEB、小程序（微信和支付宝）、Native（借助weex）ss

### 创建mpvue的项目？
```
	1、安装nodejs，基础开发
	2、安装vue脚手架 npm install --global vue-cli
	3、创建项目（ vue init mpvue/mpvue-quickstart 项目名称 ）
	4、进入项目  cd 项目名称
	5、安装依赖 （cnpm/cnpm/yarn install）
	6、启动项目 （npm run dev） 这里是把vue项目转化成小程序的项目
	7、创建新页面的时候需要js暴露，不然可能会卡死。
	8、如果出现页面未发现的错误，先重启一下查看。
	9、在for循环中，正常的顺序最好不变动，可能会出不来正常的效果。
	10、生命周期函数可以用vue的，也可以用小程序的，是兼容的。
```

**注意：导入到小程序里面，就可以在小程序中进行查看演示，但是启动程序是npm run dev，如果页面不刷新，只需在DOS命令里按下回车按钮即可，注意，开发工具打开的目录是根目录，并不是dist目录。**

### 新闻 数据接口
+ 列表数据接口
`http://www.phonegap100.com/appapi.php?a=getPortalList&catid=20&page=1`
+ 详情数据接口
`http://www.phonegap100.com/appapi.php?a=getPortalArticle&aid=496`

### 底部导航 tab切换
###### [小程序配置项官网 app.json( main.js )](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html)
```
 "tabBar": {
      "color": "black",                        //未选中的导航颜色
      "selectedColor": "#DC3925",              //选中的导航颜色
      "list": [{                               //导航菜单配置项
        "pagePath": "pages/index/main",                //导航页面路径
        "text": "首页",                                //导航文字说明
        "iconPath": "static/images/home1.png",         //未选中导航图标
        "selectedIconPath": "static/images/home2.png", //选中导航图标
      }, {
        "pagePath": "pages/user/main",
        "text": "用户",
        "iconPath": "static/images/user1.png",
        "selectedIconPath": "static/images/user2.png",
      }]
    }
```
* [x] 这里图标路径不是相对路径，不需要找路径，直接从文件夹开始找起
* [x] 小程序默认请求地址是以https开头，测试的时候可以在详情中不校验
* [ ] 请求数据的时候，用小程序的请求方法

```
 methods: {
    getData() {
      var api = 'http://www.phonegap100.com/appapi.php';
	  var _this = this;   //这里存在作用域问题
	  //小程序请求数据的方法
      wx.request({
        url: api, //仅为示例，并非真实的接口地址
        data: {
          a: "getPortalList",
          catid: "20",
          page: "1"
        },
        header: {
          "content-type": "application/json" // 默认值
        },
        success: function(res) {
          console.log(res.data);
		  _this.list = res.data.result;
        }
      });
    }
  },
  created() {
      this.getData()//自执行
  }
```
### 微信小程序 接收数据以及参数
1. 通过他自己的生命周期函数 onload钩子函数
2. 前面传值的时候通过？绑定拼接
3. 在接收数据的时候先在方法中写 requestData(){},否则会报错

## mpvue 解析html标签
[解析 html（mpvue小程序）--富文本](https://github.com/htzhanglong/mpvue-wxParse)

## mpvue解析html标签 的 富文本 插件

1. 安装   mpvue-wxparse
```npm i mpvue-wxparse  --save  /  cnpm i mpvue-wxparse  --save```
2. 引入
```import wxParse from 'mpvue-wxparse'```
3. 注册组件
```
components: {
     wxParse
}
```
4. 使用组件解析html
```<wxParse :content="article" @preview="preview" @navigate="navigate" />```
5. 引入wxparse的css
```
<style>
	@import url("~mpvue-wxparse/src/wxParse.css");
</style>
```
                              ========================后端服务配置==============================
	 
### 配置腾讯云   { [地址](https://console.qcloud.com/lav2/dev) }

1. 在开发环境中，下载测试demo，nodejs版本，非游戏的，然后解压，将解压出来的文件夹server放入到小程序的根目录
2. 找到project.config.json文件，在appid同级下，或者miniprogramRoot后面加上 "qcloudRoot": "./server/"

### 搭建本地环境

###### [搭建本地环境指南](https://cloud.tencent.com/document/product/619/11442)
	进入上述地址，然后找到   【 配置config.js 】
```
const CONF = {
      // 其他配置 ...
    serverHost: 'localhost',
    tunnelServerUrl: '',
    tunnelSignatureKey: '27fb7d1c161b7ca52d73cce0f1d833f9f5b5ec89',
      // 腾讯云相关配置可以查看云 API 秘钥控制台：https://console.cloud.tencent.com/capi
    qcloudAppId: '1257112390',
    qcloudSecretId: 'A9wXxBD6qPmyzoV2VmsnbEgw77wZ0FI9',
    qcloudSecretKey: 'AKIDIVXO3ZOKWYO2UkLjRxIRO9kCSime7n6I',
    wxMessageToken: 'weixinmsgtoken',
    networkTimeout: 30000
}
```
	 https://console.cloud.tencent.com/capi(查看qcloudSecretId与qcloudSecretKey)
     https://console.cloud.tencent.com/developer （获取密钥和appid）
	 
将上述代码复制粘贴到server目录下的config.js中相应的位置

### 配置数据库
config.js中,找到相关的数据库配置
```
mysql: {
	host: 'localhost',
	port: 3306,
	user: 'root',
	db: 'cAuth',
	pass: 'root',
	char: 'utf8mb4'
},
```


**访问数据库 mysql -uroot -p(密码)   进入数据库，然后create database cAuth;创建cAuth数据库，然后进入到server文件夹中，初始化数据库（用户登陆的一些信息等） node tools/initsb.js，之后 npm install，【server也是一个完整的项目，也需要依赖】最后启动项目，npm run dev，启动koa服务，在浏览器地址栏输入http://localhost:5757/weapp（前缀）/demo（路由） 即可访问**

##### 小程序页面配置
```language
export default {
  // 这个字段走 app.json
  config: {
  
    // 页面前带有 ^ 符号的，会被编译成首页，其他页面可以选填，
	//我们会自动把 webpack entry 里面的入口页面加进去
	
    pages: ['^pages/me/main'],
	
	**找的是js路径，开头没有 ‘/’**
    
	window: {
      backgroundTextStyle: 'light',
      navigationBarBackgroundColor: '#F22222',
      navigationBarTitleText: 'WeChat',
      navigationBarTextStyle: 'light'
    }
  }
}
```
##### 如果想批量处理代码格式问题，在package.json中，“lint”：“这里加上一个 【 --fix 】” 然后运行，npm run lint 就会处理（好像不能处理===），如果不想校验，在文件的最上面添加/* eslint-disable */，即可过滤掉本文件