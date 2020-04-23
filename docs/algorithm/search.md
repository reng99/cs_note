# 搜索算法

在列表中查找數據又有兩種方式：**順序查找**和**二分查找**。

**順序查找**適用於元素隨機排列的列表；而**二分查找**適用於元素已排序的列表，**二分查找**效率更高，但是我們必須在進行查找之前花費額外的時間將列表中的元素進行排序。

### 順序查找

對於查找數據來說，最簡單的就是從列表中的第一個元素開始對列表元素逐個進行判斷，直到找到了想要的元素，或者直到列表結尾都沒有找到。這種方法稱為`順序查找`或者`線性查找`。

這種查找的代碼實現很簡單，如下：

```javascript
/*
* @param { Array } arr 目標數組
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

當然，看到上面的代碼，如果你是個簡潔主義者，你會改寫成下面：

```javascript
const seqSearch = (arr, data) => arr.indexOf(data) >= 0
```

是的，實現的方法和體現的形式有很多種，但是原理都是一樣的：**要從第一個元素開始遍歷，有可能會遍歷到最後一個元素都找不到要查找的元素**。這是一種`暴力查找技巧`。

那麼，有什麼更加高效的查找方法嗎？有，就是下面我們要了解的~

### 二分查找算法

在開始之前，我們來玩一個`猜數字遊戲`：

- 規則：在數字`1-100`之間，你朋友選擇要猜的數字之後，由你來猜數字。你每猜一個數字，你的朋友將會作出下面的三種回應之一：
 - 才對了
 - 猜大了
 - 猜小了

這個遊戲很簡單，如果我們使用二分查找的策略進行的話，我們只需經過短短的幾次就能確定我們要查找的數據了。

那麼，**二分查找**的原理是什麼呢？

二分查找又稱為`折半查找`，對`有序的列表`每次進行對半查找~

代碼實現走一波：

```javascript
/*
* @param { Array } arr  ⚠️有序的數組
* @param { Number } data 要查找的數據
* @return { Number } 返回要查找的數據位置
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
  return -1; // 你朋友選要猜的數據在1-100範圍之外
}
```