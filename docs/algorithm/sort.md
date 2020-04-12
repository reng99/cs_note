## 排序算法

排序介紹：

- 壹旦我們將數據放置再某個數據結構（比如數組）中存儲起來後，就可以根據需求對數據進行不同方式的排序
 - 比如對姓氏按照字母
 - 比如對商品按照價格排序
 - etc

排序算法又分為`簡單排序`和`高級排序`。其中簡單排序包括`冒泡排序、選擇排序和插入排序`。高級排序包含`希爾排序、歸併排序和快速排序`。

> :warning: 這裡僅僅介紹六種排序算法

下面我們逐個了解下：

### 冒泡排序

之所以叫做`冒泡排序`，是因為使用這種排序算法時，數據值就會像氣泡那樣從數組的一端漂浮到另一端。

假設正在將一組數據按照升序排列，較大的值會浮動在數組的右側，而較小的值則會浮動到數組的左側。產生這種冒泡的現象是因為算法會多次在數組中移動，比較相鄰的數據，當左側值大於右側值得時候，將它們互換。

> 後面說到的排序算法如果沒有額外說明，則默認為升序

比如下面的簡單列表的例子。

`E A D B H`

經過第一次的排序之後，列表會變成：

`A E D B H`

前面兩個元素進行了交換。接下來再次排序：

`A D E B H`

第二個元素和第三個元素進行了交換。繼續進行排序：

`A D B E H`

第三個元素和第四個元素進行了交換。這一輪最後進行排序：

`A D B E H`

五個數據的位置沒有發生變動。因為第四個元素比最後一個元素小，所以比較後保持各自所在的位置。反復對第一個元素執行上面的操作**（已經固定的值不參與排序，如第一輪固定的`H`不參與第二輪的比較了）**，得到的最終結果：

`A B D E H`

相關的動圖如下：

![bubble](./imgs/bubble.gif "border_img_bubble")

關鍵代碼如下：

```javascript
bubbleSort() {
  let numElements = this.arr.length;
  for(let outer = numElements-1; outer >= 2; --outer) {
    for(let inner = 0; inner <= outer-1; ++inner) {
      if(this.arr[inner] > this.arr[inner+1]) {
        this.swap(inner, inner+1); // 交换数组两个元素
      }
    }
  }
}
```

### 選擇排序

`選擇排序`從數組的開頭開始，將第一個元素和其他元素進行比較。檢查完所有的元素之後，最小的元素會被放在數組的第一個位置，然後算法會從第二個位置繼續。這個過程進行到數組的倒數第二個位置時，所有的數據便完成了排序。

**原理：**

`選擇排序`依然使用雙層循環。外循環從數組的第一個元素移動到倒數第二個元素；內循環從`當前外循環指定元素的第二個元素`開始移動到最後一個元素，查找比當前外循環所指元素`小`的元素。每次循環迭代後，數組中最小的值都會被賦值到合適的位置。

下面是對五個元素的列表進行`選擇排序`的簡單例子。初始列表為：

`E A D H B`

第一次排序會找到最小值，並將它和列表的第一個元素進行交換位置：

`A E D H B`

接下來查找第一個元素後面的最小值**（第一個元素此時已經就位了）**，並對它們進行交換：

`A B D H E`

`D`已經就位了，因此下一步會對`E、H`進行交換，列表的順序排列好，如下：

`A B D E H`

通過動圖可能容易理解：

![selection](./imgs/selection.gif "border_img_selection")

關鍵的代碼如下：

```javascript
selectionSort() {
  let min,
    numElements = this.arr.length;
  for(let outer = 0; outer <= numElements-2; outer++) {
    min = outer;
    for(let inner = outer+1; inner <= numElements-1; inner++) {
      if(this.arr[inner] < this.arr[min]) {
        min = inner;
      }
    }
    this.swap(outer, min);
  }
}
```

### 插入排序

`插入排序`類似我們按照數字或字母的順序對數據進行降序或者升序排序整理~

**原理：**

插入排序也用到了嵌套循環。外循環將數組挨個移動，而內循環則對外循環中選中的元素以及內循環數組後面的那個元素進行比較。如果外循環中選中的元素比內循環中選中的元素要小，那麼內循環的數組元素會向右移動，騰出一個外置給外循環選定的元素。

上面表達得令人晦澀難懂。**簡單來說，插入排序就是未排序的元素對已經排序好的序列數據進行合適位置的插入。**如果還是難懂，結合下面的排序示例來理解下：

下面對五個元素進行插入排序。初始列表如下：

`E B A H D`

第一次插入排序，第二個元素會挪動到第一位：

`B E A H D`

第二次插入排序是對`A`進行操作：

`B A E H D`

`A B E H D`

第三次是對`H`進行操作，因為它比之前的元素都大，所以保持位置。最後一次是對`D`元素進行插入排序了，過程和最後結果如下：

`A B E D H`

`A B D E H`

結合相關的`GIF`圖了解下：

![insertion](./imgs/insertion.gif "border_img_insertion")

相關的代碼如下：

```javascript
insertionSort() {
  let temp,
    inner,
    numElements = this.arr.length;
  for(let outer = 1; outer <= numElements-1; outer++) {
    temp = this.arr[outer];
    inner = outer;
    while(inner > 0 && this.arr[inner-1] >= temp) {
      this.arr[inner] = this.arr[inner-1];
      inner--;
    }
    this.arr[inner] = temp;
  }
}
```

### 希爾排序

`希爾排序`是插入排序的優化版，但是，其核心理念和`插入排序`不同，希爾排序首先會比較距離較遠的元素，而非相鄰的元素。

**原理：**

希爾排序通過定義一個間隔序列來表示數據在排序過程中進行比較的元素之間有多遠的間隔。`我們可以動態定義間隔序列，不過對於大部分的實際應用場景，算法用到的間隔序列可以提前定義好`。

如下演示希爾排序中，間隔序列是如何運行的：

![shell_seq](./imgs/shell_seq.jpg "border_img_shell_seq")

結合下面的`GIF`圖或許會有新的體會：

![shell](./imgs/shell.gif "border_img_shell")

實現的代碼如下：

```javascript
shellSort() {
  let temp,
    j,
    numElements = this.arr.length;
  for(let g = 0; g < this.gaps.length; ++g) {
    for(let i = this.gaps[g]; i < numElements; ++i) {
      temp = this.arr[i];
      for(j = i; j >= this.gaps[g] && this.arr[j - this.gaps[g]] > temp; j -= this.gaps[g]){ // 之前的已經排好序了
        this.arr[j] = this.arr[j - this.gaps[g]];
      }
      this.arr[j] = temp; // 這裡和上面的兩個for循環是互換兩個數據位置
    }
  }
}
```

> :confused: 思考：[6, 0, 2, 9, 3, 5, 8, 0, 5, 4] 間隔為3的排序結果是什麼呢？

### 歸並排序

**原理：**

把一系列排好的子序列合併成一個大的有序序列。從理論上講，這個算法很容易實現。我們需要兩個排好序的子數組，然後通過比較數據的大小，從最小的數據開始插入，最後合併得到第三個數組。然而實際上操作數據量相當大的時候，使用歸併排序示很耗內存的，我們這裡了解下就行了。

![merge](./imgs/merge.gif "border_img_merge")

實現歸併排序一般有兩種方法，一種是**自頂向下**，另一種是**自底向上**。

上面的`GIF`圖演示的是**自頂向下**的方法，那麼，何為自頂向下呢？

`自頂向下`的歸併排序算法就是把數組元素不斷的`二分`，直到數組的元素個數為一個，因為這個時候子數組必定是有序的，然後再將兩個有序的序列合併成一個新的有序序列，又接著兩個有序序列又可以合併成一個新的有序序列，以此類推，直到合併成一個有序的數組。如下圖分解：

![merge_to_bottom](./imgs/merge_to_bottom.jpg "border_img_merge_to_bottom")

`自底向上`的歸併排序算法的思想是將數組先一個一個歸併成兩兩有序的序列，兩兩有序的序列歸併成四個四個有序的序列，以此類推，直到歸併的長度`大於`整個數組的長度，此時整個數組有序。

> :warning: 數組按照歸併長度劃分，最後一個子數組可能不滿足長度要求，這種情況要特殊處理了。

![merge_to_top](./imgs/merge_to_top.jpg "border_img_merge_to_top")

### 快速排序

`快速排序`是處理大數據集最快的排序算法之一，**時間複雜度**最好的情況也是和歸併排序一樣，為`O(nlogn)`。

**原理：**

`快速排序`是一種**分而治之（分治）**的算法，通過遞歸的方式將數據依次分解為包含較小元素和較大元素的不同子序列，然後不斷重複這個步驟，直到所有的數據都是有序的。

可以更清晰地表達`快速排序`算法的步驟如下：

1. 選擇一個基準元素**（pivot，樞紐）**，將列表分隔為兩個子序列
2. 對列表重新排列，將所有小於基準值的元素放在基準值的前面，將所有大於基準值的元素放在基準值的後面
3. 分別對較小元素的子序列和較大元素的子序列重複步驟`1和2`

![quick](./imgs/quick.gif "border_img_quick")

我們來用代碼實現下：

```javascript
// 快速排序
quickSort() {
  this.arr = this.quickAux(this.arr)
}

// aux函數 - 快排的輔助函數
quickAux(arr) {
  let numElements = arr.length；
  if(numElements === 0) {
    return []
  }
  let left = [],
    right = [],
    pivot = arr[0]; // 取數組的第一個元素作為基準值
  for(let i = 1; i < numElements; i++) {
    if(arr[i] < pivot) {
      left.push(arr[i]);
    } else {
      right.push(arr[i]);
    }
  }
  return this.quickAux(left).concat(pivot, this.quickAux(right));
}
```

以上介紹了六種排序的算法，當然還有很多其他的排序算法，你可以到[視屏 | 手撕九大經典排序算法，看我就夠了!](https://zhuanlan.zhihu.com/p/52884590)文章中查看。