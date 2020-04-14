# 搜索算法

在列表中查找数据又有两种方式：**顺序查找**和**二分查找**。

**顺序查找**适用于元素随机排列的列表；而**二分查找**适用于元素已排序的列表，**二分查找**效率更高，但是我们必须在进行查找之前花费额外的时间将列表中的元素进行排序。

### 顺序查找

对于查找数据来说，最简单的就是从列表中的第一个元素开始对列表元素逐个进行判断，直到找到了想要的元素，或者直到列表结尾都没有找到。这种方法称为`顺序查找`或者`线性查找`。

这种查找的代码实现很简单，如下：

```javascript
/*
* @param { Array } arr 目标数组
* @param { Number } data 要查找的元素
* @return { Boolean } 是否查找成功
**/
function seqSearch(arr, data){
  for(let i = 0; i < arr.length; i++){
    if(arr[i] === data){
    return true;
    }
  }
  return false;
}
```

当然，看到上面的代码，如果你是个简洁主义者，你会改写成下面：

```javascript
const seqSearch = (arr, data) => arr.indexOf(data) >= 0
```

是的，实现的方法和体现的形式有很多种，但是原理都是一样的：**要从第一个元素开始遍历，有可能会遍历到最后一个元素都找不到要查找的元素**。这是一种`暴力查找技巧`。

那麽，有什麽更加高效的查找方法吗？有，就是下面我们要了解的~

### 二分查找算法

在开始之前，我们来玩一个`猜数字游戏`：

- 规则：在数字`1-100`之间，你朋友选择要猜的数字之后，由你来猜数字。你每猜一个数字，你的朋友将会作出下面的三种回应之一：
 - 才对了
 - 猜大了
 - 猜小了

这个游戏很简单，如果我们使用二分查找的策略进行的话，我们只需经过短短的几次就能确定我们要查找的数据了。

那麽，**二分查找**的原理是什麽呢？

二分查找又称为`折半查找`，对`有序的列表`每次进行对半查找~

代码实现走一波：

```javascript
/*
* @param { Array } arr  ⚠️有序的数组
* @param { Number } data 要查找的数据
* @return { Number } 返回要查找的数据位置
**/
function binSearch(arr, data){
  let upperBound = arr.length -1,
    lowerBound = 0;
	while(lowerBound <= upperBound){
    let mid = Math.floor((upperBound + lowerBound) / 2);
    if(arr[mid] < data){
      lowerBound = mid + 1;
    }else if(arr[mid] > data){
      upperBound = mid + 1;
    }else{
      return mid;
    }
  }
  return -1; // 你朋友选要猜的数据在1-100范围之外
}
```