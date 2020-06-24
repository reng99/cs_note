# JAVASCRIPT魔法

實際場景中使用的比较频繁的`JAVASCRIPT TRICK`。

場景的目錄如下：

- <a href="#/javascript/README?id=複製文本">複製文本</a>
- <a href="#/javascript/README?id=文件上傳">文件上傳</a>
- <a href="#/javascript/README?id=數組中數字和字符串快速轉換">數組中數字和字符串快速轉換</a>
- <a href="#/javascript/README?id=生成UUID">生成UUID</a>
- <a href="#/javascript/README?id=格式化數字成金額格式">格式化數字成金額格式</a>

## 複製文本

複製功能，這裡使用[vue](https://vuejs.org/)實現！

你可以選擇使用原生的代碼編寫，如下：

```vue
<template>
  <a @click="copy('demo text')">複製</a>
  <!-- 中轉，因為select方法只對input和textarea有效 -->
  <input ref="copy" type="text" value=""/>
</template>
<script>
export default {
  methods: {
    // 複製
    copy(text) {
      this.$refs.copy.value = text
      this.$refs.copy.select() // 選中輸入框的內容
      document.execCommand('copy') // 執行瀏覽器複製命令
      console.log('複製成功！')
      window.getSelection().removeAllRanges() // 清除選中的內容
    }
  }
}
</script>
```

你也可以使用現有的優秀庫[clipboard](https://github.com/zenorocha/clipboard.js)處理，如下：

首先，你得先下載`clipboard`依賴 - 

```bash
npm install clipboard --save
或
yarn add clipboard  // 推薦使用yarn
```

為了方便我們在多個文件中使用`clipboard`，我們先在`main.js`文件裡面註冊：

```javascript
import clipboard from 'clipboard' // 引入
Vue.prototype.clipboard = clipboard // 掛載在原型上
```

在相關的文件中使用：

```vue
<template>
  <a id="mock_path">{{mockPath}}</a>
  <a ref="copy" data-clipboard-action="copy" data-clipboard-target="#mock_path" @click="copyLink">點擊複製地址</a>
</template>

<script>
export default{
  data() {
    return {
      mockPath: 'https:www.github.com/',
      copyBtn: null, // 存儲初始化複製按鈕事件
    }
  },
  mounted() {
    this.initCopy()
  },
  methods: {
    // 複製
    copyLink() {
      let vm = this;
      let clipboard = vm.copyBtn;
      clipboard.on('success', function(e) {
        console.log('複製成功！')
        vm.initCopy()
      });
      clipboard.on('error', function() {
        console.log('複製失敗！')
        vm.initCopy()
      });
    },
    // 初始化copy，防止重複觸發（這步很重要）
    initCopy() {
      let vm = this
      if(vm.copyBtn) { vm.copyBtn.destroy() }
      vm.copyBtn = new vm.clipboard(vm.$refs.copy);
    },
  }
}
</script>
```


推薦第二種方式實現！

## 文件上傳

文件上傳，分為單文件上傳和多文件上傳。

單文件的上傳比較簡單。這裡只是多文件上傳的實現：

```vue
<template>
  <a-upload :fileList="fileList" multiple :beforeUpload="() => { return false}" @change="handleChange">
    <a-button type="primary" icon="upload" style="width: 140px;">請選擇</a-button>
  </a-upload>
  <a-button type="primary" @click="upload">上傳</a-button>
</template>
<script>
export default {
  data() {
    return {
      fileList: []
    }
  },
  methods: {
    // 上傳
    upload() {
      const { fileList } = this
      if(fileList.length === 0) {
        this.$message.warning('請選擇上傳文件！')
        return
      }
      const formData = new FormData()
      fileList.map(file => {
        formData.append("file", file.originFileObj) // append 後的 file 可以跟後端約定一個字段，不一定是file
      })
      formData.append("type", "demo") // 其他的字段也是通過append添加
      // 之後 api 調用
    },
    // 文件發生改變
    handleChange({ fileList }) {
      this.fileList = fileList
    }
  }
}
</script>
```

當然，單文件上傳通過遍歷接口調用也是可以實現多文件上傳的效果~

## 數組中數字和字符串快速轉換

直接上案例：

```javascript
const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
arr.map(String);  // ['1', '2', '3', '4', '5', '6', '7', '8', '9']

const a = ['1', '2', '3', '4', '5', '6', '7', '8', '9']
a.map(Number);  // [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

## 生成UUID

`482363e9-9191-35a7-8fb0-9a7236d18e86`

[UUID的維基百科](https://zh.wikipedia.org/wiki/%E9%80%9A%E7%94%A8%E5%94%AF%E4%B8%80%E8%AF%86%E5%88%AB%E7%A0%81)

通用唯一識別碼（英文：Universally Unique Identifier，縮寫：UUID）是用於計算機體系中以識別信息數目的一個128位標誌符，還有相關的術語：全局唯一標誌符（GUID）

UUID是由一組**32位數的16進制**數字構成，故UUID理論上的總數是`1632=2128`，約等於`3.4 x 1038`，也就是說每納秒`(ns)`產生`1萬億`個UUID，要花`100億年`才會用完所有的UUID。

代碼實現如下：

```javascript
function S4() {
  return (((1+Math.random())*0x10000)|0).toString(16).substring(1); 
  // 0x10000 16進制65536  
  // ((1+Math.random())*0x10000)|0為或運算，取整
  // toString(16)轉成16進制的字符串
  // substring(1)從第二位開始截取字符
}
function guid() {
  return (S4()+S4()+"-"+S4()+"-"+S4()+"-"+S4()+"-"+S4()+S4()+S4());
}

// 直接調用
guid()
```

## 格式化數字成金額格式

在`Number`的原型上操作：

```javascript
Number.prototype.numberFormat = function(tail, decimal, thousand) {
  let n = this;
  let c = isNaN(tail = Math.abs(tail)) ? 2 : tail;
  let d = decimal == undefined ? "." : decimal;
  let t = thousand == undefined ? "," : thousand;
  let s = n < 0 ? "-" : "",
    i = String(parseInt(n = Math.abs(Number(n) || 0).toFixed(2))),
    j = (j = i.length) > 3 ? j % 3 : 0
    return s + (j ? i.substr(0, j) + t : "") + i.substr(j).replace(/(\d{3})(?=\d)/g, "$1" + t) + (c ? d + Math.abs(n - i).toFixed(c).slice(2) : "");
}
```

函數有三個參數，分別說明如下：

`tail` => 小數點後面有幾位（四捨五入到指定的位數）

`decimal` => 小數點符號（.），把它作為參數，是因為你可以自己指定所需的符號。

`thousand` => 千分位符號（,），也就是格式化成`12,345.67`的逗號，你可以設置成你需要的符號。