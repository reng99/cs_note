# 散列表/哈希表

![hash](./imgs/hash.jpg "border_img_hash")

散列是一種常用的存儲技術，散列使用的數據結構叫做**散列表/哈希表**。在散列表上`插入、刪除和取用`數據非常快，但是對於`查找`操作來說卻效率低下，比如查找一組數據中的最大值和最小值。查找的操作得求助其他的數據結構，比如下面提到的**二叉樹**。

切入一個案例來感受下哈希表：

**加入一家公司有1000個員工，現在我們需要把這些員工的信息使用某種數據結構來保存起來。你會採用什麼數據結構呢？**

- 方案一：數組
 - 按照順序將所有的員工信息依次存入一個長度為`1000`的數組中。每個員工的信息都保存在改數組的某個位置上。
 - 但是我們要查看某個員工的信息怎麼辦？一個個查找嗎？太好找。
 - 數組最大的優勢是什麼？通過下標獲取信息。
 - 所以為了可以通過數組快速定位到某個員工，最好給員工信息添加一個員工編號，而`編號`對應的就是員工的`下標值index`。
 - 當查找某個員工信息時，通過員工號可以快速定位到員工的信息位置。

- 方案二：鏈錶
 - 鏈錶對插入和刪除數據有一定的優勢
 - 但是對於獲取員工的信息，每次都必須從頭遍歷到尾，這種方式顯然不是特別適合我們這種場景。

- 最終方案
 - 這麼看來，最終方案似乎就是數組了，但是數組還是有缺點，什麼呢？
 - 假如我們想查看下張三這位員工的信息，但是我們不知道張三的員工編號，怎麼辦？
 - 當然，我們可以問他的員工編號。但是我們沒查一個員工都要問下員工的編號嗎？不合適`【PS：那我們還不如直接問他的信息得了】`
 - 能不能有一種方法，讓張三的名字和他的員工編號產生直接的關係呢？
 - 也就是通過張三這個名字，我們就能獲取到他的索引值，然後再通過索引值就能獲取張三的信息呢？
 - 這樣的方案已經存在了，就是使用**哈希函數**，讓某個`key`的信息和索引值對應起來。

那麼，下面了解下散列表的原理和實現。

我們下面的散列表是基於數組完成的，我們從數組這裡切入解析下。`數組可以通過下標直接定位到相應的空間`，哈希表的做法就是類似的實現。哈希表把`(key)鍵`通過一個固定的算法函數（此函數稱為哈希函數/散列函數）轉換成一個整型數字，然後就將該數字對數組長度進行**取餘**,取餘結果就當做數組的下標，將`(value)值`存儲在改數字為下標的數組空間裡，而當使用哈希表進行查詢的時候，就再次使用哈希函數將`key`轉換為對應的數組下標，並定位到改空間獲取`value`。

結合下面的代碼，我們會更加容易理解：

```javascript
class HashTable {
  constructor(){
    this.table = new Array(137);
  }
  /**
  * @method hashFn 哈希函數
  * @param { String } data 傳入的字符串
  * @return { Number } 返回取餘數字
  */
  hashFn(data){
    let total = 0;
    for(let i = 0; i < data.length; i++){
      total += data.charCodeAt(i);
    }
    return total % this.table.length;
  }
  /**
  * 
  * @param { String } data 傳入字符串
  */
  put(data){
    let pos = this.hashFn(data);
    this.table[pos] = data;
    return this;
  }
  // 展示
  show(){
    this.table && this.table.forEach((item, index) => {
      if(item != undefined){
        console.log(index + ' => ' + item);
      }
    })
  }
  // ...獲取值get函數等，看官感興趣的話自己補充測試下啦
}

let hashtable = new HashTable(),
  arr = ['mouse', 'ox', 'tiger', 'rabbit', 'dragon', 'snake', 'horse', 'sheep', 'monkey', 'chicken', 'dog', 'pig'];
arr.forEach(item => {
  hashtable.put(item);
});
hashtable.show();
// 5 => mouse
// 40 => dog
// 46 => pig
// 80 => rabbit
// 87 => dragon
// 94 => ox
// 111 => monkey
// 119 => snake
// 122 => sheep
// 128 => tiger
// 134 => horse

// 那麼問題來了，十二生肖裡面的_小雞_去哪裡了呢❓
// 被_小狗_給覆蓋了，因為其位置也是40（這個可以自己證明下）
// 問題又來了，那麼應該如何解決這種被覆蓋的衝突呢❓
```

針對上面的問題，我們存儲數據的時候，產生衝突的話，可以像下面這樣解決：

**1. 線性探測法**

當發生`碰撞（衝突）`時，線性探測法檢測散列表中的下一個位置`【有可能是非順序查找位置，不一定是下一個位置】`是否為空。如果為空，就將數據存入該位置；如果不為空，則繼續檢查下一個位置，直到找到下一個空的位置為止。該技術是基於一個事實：**每個散列表都有很多空的單元格，可以使用它們來存儲數據。**

**2. 開鏈法**

當發生`碰撞（衝突）`時，我們仍然希望將`key（鍵）`存儲到通過哈希函數產生的索引位置上，那麼我們可以使用**開鏈法**。**開鏈法**是指實現哈希表底層的數組中，每個數組元素又是一個新的數據結構，比如是另一個數組`（這樣結合起來就是二維數組了）`,鏈表等，這樣就能存儲多個鍵了。使用這種技術，即使兩個`key（鍵）`散列後的值相同，依然是被保存在相同的位置，只不過它們是被保存在另一個數據結構上而已。以另一個數據結構是數組為例，存儲的數據如下：

![hash_open_linked](./imgs/open_linked.jpg "border_img_open_linked")







