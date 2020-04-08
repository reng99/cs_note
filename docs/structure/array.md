# 數組

![array](./imgs/array.jpg "border_img_array")

數組是一種`線性結構`，以中國年生肖為例，其排序固定為`鼠、牛、虎、兔、龍、蛇、馬、羊、猴、雞、狗、豬`。

我們來創建一個數組並且打印下結果就清晰了：

```javascript
let arr = ['鼠', '牛', '虎', '兔', '龍', '蛇', '馬', '羊', '猴', '雞', '狗', '豬'];
arr.forEach((item, index) => {
	console.log(`[ ${index} ] => ${item}`);
});

// [ 0 ] => 鼠
// [ 1 ] => 牛
// [ 2 ] => 虎
// [ 3 ] => 兔
// [ 4 ] => 龍
// [ 5 ] => 蛇
// [ 6 ] => 馬
// [ 7 ] => 羊
// [ 8 ] => 猴
// [ 9 ] => 雞
// [ 10 ] => 狗
// [ 11 ] => 豬
```

> :warning: 啰嗦下：數組的下標是從 **0** 開始的

數組中常用的屬性和一些方法，直接調用即可。

## 常用屬性

- **length** 表示數組的長度

一般用做條件判斷，顯示或者隱藏某些內容。

## 常用方法

- **splice(index, howmany, item, ...itemx)**

`splice`方法自認為是數組中最強大的方法。可以實現數組的添加、刪除和替換。

- **indexOf(searchValue, formIndex)**

`indexOf`方法返回某個指定字符串在數組中的位置。`lastIndexOf`有異曲同工之妙，不過是從數組尾部開始搜索。

- **concat(array1, ...arrayn)**

`concat`方法用於連接兩個或以上的數組。

- **push(newElement1, ...newElementN)**

`push`方法可向**數組末尾**添加一個或者多個元素。

- **unshift**