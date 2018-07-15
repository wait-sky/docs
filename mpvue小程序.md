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