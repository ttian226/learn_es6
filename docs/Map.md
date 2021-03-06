### [Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)

Map和对象(Object)的区别是：Map的键可以是任意类型的值，而对象的键只能是字符串（如果不是字符串会自动转换为字符串）

```javascript
var m = new Map();
var o = {p: 'hello world'};

// 这里对象o作为m的一个键
m.set(o, 'content');
m.get(o); //'content'

m.has(o); //true
m.delete(o);  //true
m.has(o); //false
```

Map接受一个数组作为参数，该数组的成员是一个个表示键值对的数组。

```javascript
var kvArray = [
  ['key1', 'value1'], 
  ['key2', 'value2']
];
var m = new Map(kvArray); //Map {"key1" => "value1", "key2" => "value2"}

m.size; //2
m.has('key1');  //true
m.get('key1');  //'value1'
m.has('key2');  //true
m.get('key2');  //'value2'
```

用数组初始化Map等价于下面的代码

```javascript
var kvArray = [
  ['key1', 'value1'],
  ['key2', 'value2']
];

kvArray.forEach(function ([key, value]) {
  m.set(key, value);
});

// 或者使用箭头函数简写成
kvArray.forEach(([key, value]) => m.set(key, value));

//Map {"key1" => "value1", "key2" => "value2"}
```

如果对同一个键多次赋值，后面的值将覆盖前面的值。

```javascript
let map = new Map();

map.set(1, 'aaa').set(1, 'bbb');

map.get(1); //'bbb'
```

如果读取一个未知的键，则返回undefined。

```javascript
new Map().get('asfddfsasadf')
// undefined
```

只有对同一个对象的引用，Map结构才将其视为同一个键。
如果键是对象，Map的键是和它的内存地址绑定的，只要内存地址不一样，就视为两个键。
如果键是简单类型的值（数值，字符串，布尔值），则只要两个值严格相等就视为一个键。（尽管NaN !== NaN，但Map视为同一个键）

```javascript
// 键是数值，同名的会覆盖
let map1 = new Map();
map1.set(1, 111);
map1.set(1, 222);
console.log(map1.size); //1

// 键是字符串，同名会覆盖
let map2 = new Map();
map2.set('a', 111);
map2.set('a', 222);
console.log(map2.size); //1

// 键是数组，数组也是对象，虽然都是['a']但是它们的内存地址不同，两次添加不会被覆盖
let map3 = new Map();
map3.set(['a'], 111);
map3.set(['a'], 222);
console.log(map3.size); //2

// 键是数组，添加同一个数组的引用（内存地址），会被覆盖
let map4 = new Map();
let arr = ['a'];
map4.set(arr, 111);
map4.set(arr, 222);
console.log(map4.size); //1

// 键是对象，它们的内存地址不同，两次添加不会被覆盖
let map5 = new Map();
map5.set({a: 1}, 111);
map5.set({a: 1}, 222);
console.log(map5.size); //2

// 添加相同对象的引用，会被覆盖
let map6 = new Map();
let o = {a: 1};
map6.set(o, 111);
map6.set(o, 222);
console.log(map6.size); //1
```

**Map实例的属性和方法**

1. `size`属性，返回Map结构的成员总数

```javascript
let map = new Map();
map.set('foo', true);
map.set('bar', false);

map.size // 2
```

2. `set(key, value)`，添加一个键值对作为Set的成员，并返回这个Map。如果已经有相同的key，值会被更新。

```javascript
var m = new Map();

m.set("edition", 6)        // 键是字符串
m.set(262, "standard")     // 键是数值
m.set(undefined, "nah")    // 键是undefined
```

由于Set返回的是Map本身，所以可以链式调用。

```javascript
let map = new Map()
  .set(1, 'a')
  .set(2, 'b')
  .set(3, 'c');
```

3. `get(key)`，读取key对应的键值，如果找不到key，返回undefined

```javascript
var m = new Map();

var hello = function() {console.log("hello");}
m.set(hello, "Hello ES6!") // 键是函数

m.get(hello)  // Hello ES6!
```

4. `has(key)`，表示某个键是否在Map数据结构中

```javascript
var m = new Map();

m.set("edition", 6);
m.set(262, "standard");
m.set(undefined, "nah");

m.has("edition")     // true
m.has("years")       // false
m.has(262)           // true
m.has(undefined)     // true
```

5. `delete(key)`，删除某个键，删除成功返回true，删除失败返回false。

```javascript
var m = new Map();

m.set(1, 'a').set(2, 'b');

m.delete(1);  //true 此时Map {2 => "b"}
m.delete(1);  //false
```

6. `clear()`，清除所有成员，没有返回值

```javascript
let map = new Map();
map.set('foo', true);
map.set('bar', false);

map.size // 2
map.clear()
map.size // 0
```

