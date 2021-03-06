### 静态方法

#### [Array.from()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from)

1. 类数组对象（array-like）
2. 可遍历的对象（iterable）
把上面两类的对象转换为真正的数组

```javascript
let arrayLike = {
  '0': 'a',
  '1': 'b',
  '2': 'c',
  length: 3
};

// es5写法
var arr1 = [].slice.call(arrayLike);  //[1, 2, 3]

// es6写法
var arr2 = Array.from(arrayLike); //[1, 2, 3]
```


遍历DOM对象

```html
<p>1</p>
<p>2</p>
<p>3</p>
```

```javascript
document.addEventListener('DOMContentLoaded', function () {
  // querySelectorAll返回的是NodeList对象，一个类数组对象。
  var doms = document.querySelectorAll('p');

  // 转换为数组
  var domArr = Array.from(doms);

  // 调用数组实例方法Array.prototype.forEach
  domArr.forEach(function (item) {
    console.log(item);
  });

  // 注，doms.forEach也不会报错，它实际上是调NodeList.prototype.forEach方法（继承了数组的原型方法）
  
  // 取前两个dom对象，调用Array.prototype.slice方法
  var subdoms = domArr.slice(0, 2);

  // 报错，因为NodeList对象没有原型方法slice
  var subdoms = doms.slice(0, 2);
});
```

处理arguments对象

```javascript
function foo() {
  // 通过Array.prototype.slice转换成数组
  var args = Array.prototype.slice.call(arguments);

  // 通过Array.from转换成数组
  var args = Array.from(arguments);

  args.forEach(function (item) {
    console.log(item);
  });
}

foo(1, 2, 3);
```

把Set对象（iterable对象）转换为数组

```javascript
var items = new Set([1, 2, 3, 4, 5]);
Array.from(items);  // [1, 2, 3, 4, 5]
```

`Array.from`还可以接受一个回调函数作为第二个参数，作用类似于数组的`map`方法，用来对每个元素进行处理，将处理后的值放入返回的数组。

```javascript
var set = new Set([2, 3, 4]);

var arr1 = Array.from(set).map(i => i * 2); //[4, 6, 8]
// 等价于上面方法
var arr2 = Array.from(set, i => i * 2); //[4, 6, 8]
```

#### [Array.of()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/of)

Array.of()用于将一组值转换为数组

```javascript
var arr1 = Array.of(1, 2, 3); //[1, 2, 3]
var arr2 = Array.of(1); //[1]
var arr3 = Array.of();  //[]
var arr4 = Array.of(undefined); //[undefined]
```

可以用下面这个方法来替代Array.of()功能

```javascript
function ArrayOf() {
  return [].slice.call(arguments);
}
```

#### [Array.prototype.copyWithin()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/copyWithin)

在当前数组内部，将指定位置的成员复制到其他位置（会覆盖原有成员），然后返回当前数组。

`Array.prototype.copyWithin(target, start = 0, end = this.length)`

从target位置开始替换数据，从start位置读取数据直到end之前的位置

```javascript
// 从位置2开始读取数据到数组结尾（第三个参数默认是数组长度，这里是5，也就是位置5之前的位置4），这里读取的数据为3, 4, 5，从位置0开始替换
var arr1 = [1, 2, 3, 4, 5].copyWithin(0, 2); //[3, 4, 5, 4, 5]
// 等价于上面的表达式，这里第三个参数显示为5，即数组的长度
var arr2 = [1, 2, 3, 4, 5].copyWithin(0, 2, 5); //[3, 4, 5, 4, 5]
// 取位置2到位置4之前的数据，即3,4
var arr3 = [1, 2, 3, 4, 5].copyWithin(0, 2, 4); //[3, 4, 3, 4, 5]
```

#### [Array.prototype.find()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find)

接受一个回调函数作为参数，返回数组中满足条件的第一个元素。

```javascript
var val1 = [1, 4, -5, -10].find(function (n) {
  return n < 0;
});

// -5

// 使用箭头函数作为回调函数
var val2 = [1, 4, -5, -10].find(n => n < 0);  // -5
```


#### [Array.prototype.findIndex()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex)

接受一个回调函数作为参数，返回数组中满足条件的第一个元素的索引（位置）

```javascript
var index1 = [1, 5, 10, 15].findIndex(function (n) {
  return n > 9;
});

// 2

var index2 = [1, 5, 10, 15].findIndex(n => n > 9); // 2
```

#### [Array.prototype.fill()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/fill)

使用给定值填充一个数组

```javascript
['a', 'b', 'c'].fill(7); //[7, 7, 7]

new Array(5).fill(0);  //[0, 0, 0, 0, 0]
```

fill方法还可以接受第二个和第三个参数，用于指定填充的起始位置和结束位置。

```javascript
['a', 'b', 'c'].fill(7, 1, 2);  //["a", 7, "c"]
```

**以下这些方法返回遍历器对象，包括：**
* `Array.prototype.keys()`
* `Array.prototype.values()`
* `Array.prototype.entries()`

#### [Array.prototype.keys()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/keys)

对键名的遍历(数组索引)

```javascript
let arr = ['a', 'b', 'c'];
for (let index of arr.keys()) {
  console.log(index);
}
// 0 1 2
```

#### [Array.prototype.values()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/values)

对键值的遍历(数组元素)

```javascript
let arr = ['a', 'b', 'c'];
for (let index of arr.values()) {
  console.log(index);
}
// 'a' 'b' 'c'
```

#### [Array.prototype.entries()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/entries)

对键值对的遍历

```javascript
let arr = ['a', 'b', 'c'];
for (let [index, elem] of arr.entries()) {
  console.log(index, elem);
}

// 0 'a'
// 1 'b'
// 2 'c'
```


