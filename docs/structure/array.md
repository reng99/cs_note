# 数组

![array](./imgs/array.jpg "border_img_array")

数组是一种`线性结构`，以中国年生肖为例，其排序固定为`鼠、牛、虎、兔、龙、蛇、马、羊、猴、鸡、狗、猪`。

我们来创建一个数组并且打印下结果就清晰了：

```javascript
let arr = ['鼠', '牛', '虎', '兔', '龙', '蛇', '马', '羊', '猴', '鸡', '狗', '猪'];
arr.forEach((item, index) => {
	console.log(`[ ${index} ] => ${item}`);
});

// [ 0 ] => 鼠
// [ 1 ] => 牛
// [ 2 ] => 虎
// [ 3 ] => 兔
// [ 4 ] => 龙
// [ 5 ] => 蛇
// [ 6 ] => 马
// [ 7 ] => 羊
// [ 8 ] => 猴
// [ 9 ] => 鸡
// [ 10 ] => 狗
// [ 11 ] => 猪
```

> :warning: 啰嗦下：数组的下标是从 **0** 开始的

数组中常用的属性和一些方法，直接调用即可。

## 常用属性

- **length** 表示数组的长度

一般用做条件判断，显示或者隐藏某些内容。

## 常用方法

- **splice(index, howmany, item, ...itemx)**

`splice`方法自认为是数组中最强大的方法。可以实现数组的添加、删除和替换。

- **indexOf(searchValue, formIndex)**

`indexOf`方法返回某个指定字符串在数组中的位置。`lastIndexOf`有异曲同工之妙，不过是从数组尾部开始搜索。

- **concat(array1, ...arrayn)**

`concat`方法用于连接两个或以上的数组。

- **push(newElement1, ...newElementN)**

`push`方法可向**数组末尾**添加一个或者多个元素。

- **unshift(newElement1, ...newElementN)**

`unshift`方法可以向**数组开头**添加一个或者多个元素。

- **pop()**

`pop`方法用于删除并返回**数组中的最后一个元素**。

- **shift()**

`shift`方法用于删除并返回**数组的第一个元素**。

- **reverse()**

`reverse`方法用于数组的反转。

- **sort(sortFn)**

`sort`方法是对数组元素排序。参数`sortFn`可选，用于规定排序的顺序（升序抑或降序），必须是函数。

```javascript
let values = [0, 1, 5, 10, 15]
values.sort()
console.log(values) // [0, 1, 10, 15, 5]

// ❓为什麽会出现这种排序结果呢？
// ❓不是默认是升序？
// 在忽略sortFn的情况下，元素会按照装换为字符串的各个字符串的Unicode位点进行排序，如下
let equalValues = ['0', '1', '5', '10', '15']
equalValues.sort()
console.log(equalValues) //  ["0", "1", "10", "15", "5"]

// equalValues.sort()等同下面的代码
let arr = [1, 0, 10, 15, 5]
const compare = (el1, el2) => el1 >= el2 ? 1 : -1 // 升序排列
arr.sort(compare)
console.log(arr) // [0, 1, 5, 10, 15]

// 再来个降序的操作
arr.sort((el1, el2) => el2 >= el1 ? 1 : -1) // 降序排序
console.log(arr); // [15, 10, 5, 1, 0]
```

- **forEach(fn(currentValue, index, arr), thisValue)**

`forEach`方法用于调用数组的每个元素，并将元素传递给回调函数。

- **every(fn(currentValue, index, arr), thisValue)**

`every`方法用于检测数组中所有元素是否符合指定条件，如果数组中检测到有一个元素不满足，则整个表达式返回`false`，且剩余的元素不再检查。如果所有的元素都满足条件，则返回`true`。

- **some(fn(currentValue, index, arr), thisValue)**

`some`方法用于检测数组中元素是否满足指定条件。只要有一个符合就返回`true`，剩余的元素不再检查。如果所有元素都不符合条件，则返回`false`。

- **reduce(fn(accumulator, currentValue, currentIndex, arr), initialValue)**

`reduce`方法接收一个函数作为累加器，数组中的每个值（从左往右）开始缩减，最终一个值。回调函数的四个参数的意义如下：`accumulator`必需，累加器累计回调的返回值，它是上一次上次调用时返回的累计值，或者`initialValue`；`currentValue`，必需，数组中正在处理的元素；`currentIndex`，可选，数组中正在处理的当前元素的索引，如果提供了`initialValue`，则索引号为0，否则为1；`arr`,可选，当前元素所属的数组对象。`initialValue`，可选，传递给函数的初始值。

```javascript
let arr = [1, 2, 3, 4]
const reducer = (accumulator, currentValue) => accumulator + currentValue

// 1 + 2 + 3 + 4
console.log(arr.reduce(reducer)); // 10

// 5 + 1 + 2 + 3 + 4
console.log(arr.reduce(reducer, 5)); // 15
```

与数组相关的业务上使用这些属性和方法，能够`hold`住大多数业务了。

> PS：理解和熟练使用数组很重要，因为后面讲到的数据结构或多或少都有数组的影子