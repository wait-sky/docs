# JavaScript   -  String

++[ MDN - String](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String)++

#### 常见的操作方法 ~~【valueOf，includes，localeCompare，search，trim】~~

##### ** 1. charAt( ) / charCodeAt( ) **

- 说明： charAt根据索引获取指定位置的字符，charCodeAt不仅仅获取字符，他获取的是字符对应的Unicode编码值（ASCⅡ）
```js
	var str = 'zhufengpeixuna'
	str.charAt(0) / str[0] 用法是一样的
	str.charAt(100) --> "" / str[100] --> undefined
	str.charCodeAt(13) --> 97 [a => 97]
	String.fromCharCode(20013) --> 中   // 将Unicode码的值转换为字符
```
*****

##### **2. indexOf( ) / lastIndexOf( ) **

- 说明：基于这两个方法，可以获取字符在字符串中第一次或者最后一次出现的位置，有这个字符，返回大于等于零的索引，否则返回-1，所以基于这个方法可以验证当前字符串中是否包含某个字符
```js
	var str = 'zhufeng@peixuna'
	str.indexOf('@') // 7
	str.indexOf('#') // -1
```

*****

##### **3. slice( ) **

- 说明：str.slice(n, m)从索引n开始找到索引为m处，不包含m，把找到的字符串当作新的字符串返回
```js
	var str = 'zhufeng@peixuna'
	str.slice(2,7)  // "ufeng"
	str.slice(2)   // "ufeng@peixuna"
	str.slice()  // "zhufeng@peixuna"
	str.slice(0)  // "zhufeng@peixuna"
	str.slice(-3, -1)  // "un"
```
> **和数组中的slice方法是一样的**

*****

##### ** 4. substr( ) / substring( )**

- 说明：substring语法和slice语法一模一样，唯一的区别，它不支持负数索引，substr和slice用法一样，**==但第二个参数是个数的意思，表示截取多少个，而不是索引==**
```js
	var str = 'zhufeng@peixuna'
	str.substr(3,4)  // "feng"
```

*****

##### ** 5. toUpperCase( ) / toLowerCase( ) **

- 说明：字母大小写转换，对应顺序  >  小写转大写，大写转小写
```javascript
	var str = 'zhufeng@PEIxuna'
	str.toUpperCase()  // "ZHUFENG@PEIXUNA"
	str.toLowerCase()  // "zhufeng@peixuna"
```

*****

##### ** 6. split( )**

- 说明：和数组中的join相对应，数组中的join是把数组中的每一项按照指定的连接符变为字符串，而split是吧字符串按照指定的分隔符，拆分成数组中的每一项
```javascript
	var str = 'zhufeng@PEIxuna'
	str.split('@')  // ["zhufeng", "PEIxuna"]
```
*****

##### ** 7. replace( )**

- 说明：替换字符串中的原有字符str.replace(原有字符，新字符)
```js
	var str = 'zhufeng@PEIxun@a'
	// 只能换第一次出现的位置
	str.replace('@', '-')	 // "zhufeng-PEIxuna"
	// 匹配所有的出现的位置
	str.replace(/@/g, '-')   // "zhufeng-PEIxun-a"
```
** - - - - - - - - - **

#### ** 项目引用实战 **
** 1. 将 【 2019-3-4 14:39:38 】 转换成为 【 03月04日 14时39分 】 **
>  == eg ： 抛砖引玉 ==
```js
	var str = "2019-3-4 14:39:38";
	var arr = str.split(' '); // ["2019-3-4", "14:39:38"]
	var left = arr[0].split('-'); // ["2019", "3", "4"]
	var right = arr[1].split(':'); // ["14", "39", "38"]
	function toFixed (str) {
		return str < 10 ? '0' + str : str;
	};
	var month = toFixed(left[1]);
	var day = toFixed(left[2]);
	var hour = toFixed(right[0]);
	var minite = toFixed(right[1]);
	var result = month + '月' + day + '日 ' + hour + '时' + minite + '分';
	console.log('result -> [ ', result, ' ]');  // result -> [  03月04日 14时39分  ]
```
>
>  ** == eg ： 一个有逼格的模板 == **
```js
	// 正则封装
	~ function (prototype) {
	  prototype.formatTime = function (template) {
		template = template || '{0}年{1}月{2}日 {3}时{4}分{5}秒';
		var ary = this.match(/\d+/g);
		template = template.replace(/\{(\d+)\}/g, function () {
		  console.log(arguments)
		  var n = arguments[1],
			val = ary[n] || 0;
		  val < 10 ? val = '0' + val : null; // val
		  return val;
		});
		return template;
	  }
	} (String.prototype)
	var str = '2019-3-4 14:39:38'
	// console.log(str.formatTime('{0}-{1}-{2}'))
	console.log(str.formatTime('{0}-{1}-{2} {3}:{4}:{5}'))
```

** 2. 解析地址  将 【 name=lisi&age=12 】 的形式转换成 【 {name: lisi, age: 12} 】 的形式**
>  == eg ： 抛砖引玉 ==
```javascript
	var str = 'https://www.baidu.com/s?ie=utf-8&wd=md&name=type#teacher';
    var indexASK = str.indexOf('?'),
        indexWell = str.indexOf('#'); // 他只是个锚点
    if (indexWell > -1) {
      str = str.substring(indexASK + 1, indexWell)
    } else {
      str = str.substr(indexASK + 1)
    }
    console.log('str ', str) // ie=utf-8&wd=md&name=type
    var ary = str.split('&'), // ["ie=utf-8", "wd=md", "name=type"]
        obj = {};
    for (var i = 0; i < ary.length; i++) {
      var item = ary[i], itemAry = item.split('=');
      obj[itemAry[0]] = itemAry[1]
    }
    console.log('obj ', obj)
```
>
>  ** == eg ： 一个有逼格的模板 == **
```js
	// 正则封装
	~ function (prototype) {
	  prototype.queryURLParams = function () {
		var obj = {}, reg = /([^?=&#]+)(?:=([^?=&#]+)?)/g;
		this.replace(reg, function () {
		  var key = arguments[1], value = arguments[2] || null;
		  obj[key] = value;
		});
		return obj;
	  }
	}(String.prototype)
	var str = 'https://www.baidu.com/s?ie=utf-8&wd=md&name=type#teacher';
	console.log(str.queryURLParams());
```
*****
