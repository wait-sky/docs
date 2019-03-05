# JavaScript   -  Math

++[ MDN - Math](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math)++


##### ** 1. abs( ) **

- 说明： 取绝对值
```js
	Math.abs(10)  // 10
	Math.abs(-10) // 10
```
*****

##### **2. ceil( ) / floor( ) **

- 说明：向上或者向下取整
```js
	Math.ceil(10) // 10
	Math.ceil(10.01) // 11
	Math.ceil(-10.01) // -10
	Math.floor(10.999) // 10
	Math.floor(-10.999) // -11
```

*****

##### **3. round( ) **

- 说明：四舍五入
```js
	Math.round(10.49) // 10
	Math.round(10.5) //11
	Math.round(-10.49) // -10
	Math.round(-10.5) // -10 临界点
	Math.round(-10.51) // -11
```

*****

##### ** 4. sqrt( ) **

- 说明：开平方
```js
	Math.sqrt(100)  // 10
	Math.sqrt(10)   // 3.1622776601683795
	Math.sqrt(16)   // 4
	Math.sqrt(2)    // 1.4142135623730951
```

*****

##### ** 5. pow( ) **

- 说明：取幂（N的M次方）
```javascript
	Math.pow(2, 10) // 1024
	Math.pow(3, 3) // 27
	Math.pow(4, 2) // 16
```

*****

##### ** 6. max( ) / min( ) **

- 说明：获取最大值最小值
```javascript
	Math.max(23,45,32,456,32,12,3,567,344,324)  // 567
	Math.min(23,45,32,456,32,12,3,567,344,324)  // 3
```
*****

##### ** 7. random( )**

- 说明：获取0~1之间的随机小数
```js
	for (var i = 0; i < 10; i++) {
	console.log(Math.random())
	}
	VM684:2 -> 0.9254850720499863
	VM684:2 -> 0.6966365050938694
	VM684:2 -> 0.032141844974931555
	VM684:2 -> 0.3581116058664495
	VM684:2 -> 0.9543107337574239
	VM684:2 -> 0.17975920526875377
	VM684:2 -> 0.9281517527164242
	VM684:2 -> 0.5935224687830085
	VM684:2 -> 0.574097264824502
	VM684:2 -> 0.3383567722477314
```
** - - - - - - - - - **

#### ** 项目引用实战 **
** 获取n~m之间的随机正数 **
>  == 公式 ： 【 Math.round(Math.random()*(m-n)+n) 】 ==
>  eg：求 23~45之间的正数
```js
	for (var i = 0; i < 10; i++) {
		console.log(Math.round(Math.random()*(45-23)+23))
	}
	VM751:2 -> 26 33 29 24 38 28 40 40 23 37
```

