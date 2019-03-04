# JavaScript   -   Aarry

#### 常见的操作方法 ~~【pop，push，shift，unshift】~~

##### ** 1. splice( )**

- 说明： 返回一个新的数组，原数组中被操作的部分，没有返回[],可以拆分数组
```json
	{
		i ：'找到操作元素的坐在位置，下标'
		n ：'被删除几个元素（个数）'
		m, x, y... ：'添加多少个元素，在第i个元素之前'
	}
```

- eg： **splice(i, n, (x, y, z...))* ***

> 释义：

- [x] splice(i , n) - > 删除数组中指定位置的元素
```javascript
	var arr = [12, 23, 34, 45, 56, 67];
	var newArr = arr.splice(2, 1);
	newArr => [34];
	arr => [12, 23, 45, 56, 67];
```

- [x]splice(i, n, m) -> 替换数组中指定位置的元素
```javascript
	var arr = [12, 23, 34, 45, 56, 67];
	var newArr = arr.splice(2, 1, 100);
	newArr => [34];
	arr => [12, 23, 100, 45, 56, 67];
```

- [x] splice(i, n, m, x, y...) -> 添加某个，某些元素到指定位置元素的前面
```javascript
	var arr = [12, 23, 34, 45, 56, 67];
	var newArr = arr.splice(3, 0, 100, 12);
	newArr => [];
	arr => [12, 23, 34, 100, 12, 45, 56, 67];
```

*****

##### **2. slice( )**

- 说明：返回一个新的数组对象，这一对象是一个由 begin和 end决定的原数组的浅拷贝。原始数组不会被改变
```json
{
	i : '从下表为i的元素开始操作，范围[i, ...)'，为0或者不写，实现克隆，堆空间不一样，相互独立,
	j : '截取到下标为j的元素之前的所有元素，不写，则截取后面所有',
	说明 ： 如果i，j为负数，规则：数组总长度+负数索引
}
```

- eg： ** slice(i, j)**

> 释义：

- [x] [ ].slice(i, j) -> 截取原数组中的某些元素，到下标为j的前面，所有元素，范围[i, j)
```javascript
	var arr = [12, 23, 34, 45, 56, 67];
	var newArr = arr.slice(2, 4);
	newArr => [34, 45];
	arr => [12, 23, 34, 45, 56, 67];
```

- [x] [ ].slice(i) -> 截取原数组中的下标为i的元素及后面所有的元素
```javascript
	var arr = [12, 23, 34, 45, 56, 67];
	var newArr = arr.slice(2);
	newArr => [34, 45, 56, 67];
	arr => [12, 23, 34, 45, 56, 67];
```

- [x] [ ].slice(0)/slice() -> 原数组的浅拷贝
```javascript
	var arr = [12, 23, 34, 45, 56, 67];
	var newArr = arr.slice(0);
	newArr => [12, 23, 34, 45, 56, 67];
	arr => [12, 23, 34, 45, 56, 67];
```

- [x] [ ].slice(-3, -1) -> 从后面向前截取
```javascript
	var arr = [12, 23, 34, 45, 56, 67];
	var newArr = arr.slice(-3, -1);
	newArr => [45, 56];
	arr => [12, 23, 34, 45, 56, 67];
```

*****

##### **3. concat( )**

- 说明：方法用于合并两个或多个数组，此方法不会更改现有数组，而是返回一个新数组。
```json
{
	value : '指定的值',
	array : '数组',
	说明 ： 顺序不会变，谁在前，就在前
}
```

- eg： ** concat(value, arr)**

> 释义：

- [x] [ ].concat(value, array) -> 将已有的数组添加属性值value，数组array，返回一个新的数组
```javascript
	var newArr = [].concat('concat', [1, 2, 3])
	newArr => ["concat", 1, 2, 3]
```
**※ 他是有顺序的**

*****

##### ** 4. join( )**

- 说明：将一个数组（或一个类数组对象）的所有元素连接成一个字符串并返回这个字符串
```json
{
	join : '转换成为字符串，参数可做拼接',
	toString : '一样，但是只能转成字符串'
}
```

- eg： ** join(‘x’)**

> 释义：

- [x] join(‘x’) -> 转换成以x作为拼接连接的字符串
```javascript
	var newArr = [1, 2, 3].join('&')
	newArr => "1&2&3"
```

- [x] join(‘x’) -> 当元素都是数字或者数字类的字符串可以解析表达式
```javascript
	var newArr = eval([1, 2, 3].join('+'))
	newArr => 6
```

*****

##### ** 5. reverse( )**

- 说明：将数组中的元素顺序反过来展示，返回一个新数组

- eg： ** [ ].reverse()**

> 释义：

- [x][ ].reverse() ->将数组中的元素顺序反过来展示
```javascript
	var newArr = [1, 2, 3].reverse()
	newArr => [3, 2, 1]
```

*****

##### ** 6. sort( )**

- 说明：将数组中的元素排序，返回新的数组，顺序是根据字符串Unicode码点

- eg： ** [ ].sort()**

> 释义：

- [x] [ ].sort() -> 将数组中的元素排序

> 示例：
```javascript
	var newArr = [4, 2, 3].sort()
	newArr => [2, 3, 4]
```

- [x] [ ].sort() -> 将数组中的元素排序

> 示例：
```javascript
	var newArr = [4, 12, 13].sort()
	newArr => [12, 13, 4]
```
> 显然不是我们想要的效果，需要做一下处理：如下
```javascript
	var newArr = [4, 12, 13]
	newArr.sort(function (a, b) {
		return a - b // 升序，a -> b
	})
	newArr => [4, 12, 13]
```

*****

##### ** 7. indexOf( )**

- 说明：返回在数组中可以找到一个给定元素的第一个索引，如果不存在，则返回-1

- eg： ** [ ].indexOf(a)**

> 释义：

- [x][ ].indexOf( ) ->将数组中的元素顺序反过来展示
```javascript
	// 找到指定元素出现的位置
	var array = [2, 5, 9];
	array.indexOf(2);     // 0
	array.indexOf(7);     // -1
```
> [ ** 项目中的实例，判断一个元素是不是在数组里 **  ]
- ```javascript
	// 判断一个元素是否在数组里，不在则更新数组
	function updateFruits (fruits, fruit) {
		if (fruits.indexOf(fruit) === -1) {
			fruits.push(fruit);
			console.log('New fruits collection is : ' + fruits);
		} else if (fruits.indexOf(fruit) > -1) {
			console.log(fruit + ' already exists in the fruits collection.');
		}
	}
	var fruits = ['banana', 'orange', 'apple', 'pear'];
	updateFruits(fruits, 'peach ');
	updateFruits(fruits, 'peach ');
```

** - - - - - - - - - **

### ** 经典案例  --  【 数组去重 】 **
- 方法一：
```js
var ary = [1, 2, 3, 2, 2, 3, 4, 3, 4, 5];
let obj = {}
for (let i = 0; i < ary.length; i++) {
  const item = ary[i]; // => 每一次循环从数组中拿出来的一项
  // => 存储之前需要做判断：如果对象中已经存在这个属性了，说明当前item在之前出现过，也就是重复项，删掉
  if (typeof obj[item] !== 'undefined') {
    ary.splice(i, 1);
    i--; // => 防止数组塌陷
    /**
     * 如果数组很长，这种方式后面的索引都要重新计算，消耗性能
     */
  }
  obj[item] = item;
}
console.log(ary);
```

- 方法二：
```js
var ary = [1, 2, 3, 2, 2, 3, 4, 3, 4, 5];
let obj = {}
for (let i = 0; i < ary.length; i++) {
  const item = ary[i]; // => 每一次循环从数组中拿出来的一项
  // => 存储之前需要做判断：如果对象中已经存在这个属性了，说明当前item在之前出现过，也就是重复项，删掉
  // 这样有一个排序的错乱的问题， 需要后期酌情处理，但优于方案一
  if (typeof obj[item] !== 'undefined') {
    ary[i] = ary[ary.length - 1];
    // 去除数组中的最后一项
    ary.length--;
    i--;
    continue; // 跳出本次循环，继续下一次
  }
  obj[item] = item;
}
console.log(ary.sort());
```
- 方案三：es6最优解
```js
var ary = [1, 2, 3, 2, 2, 3, 4, 3, 4, 5]
arr = Array.from(new Set(ary))
console.log(arr)
```