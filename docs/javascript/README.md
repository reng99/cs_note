# JAVASCRIPT魔法

實際場景中使用的比较频繁的`JAVASCRIPT TRICK`。

場景的目錄如下：

- <a href="#/javascript/README?id=複製文本">複製文本</a>
- <a href="#/javascript/README?id=文件上傳">文件上傳</a>

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