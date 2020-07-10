# HTML相關概念

### HTML中的塊級標籤和行級標籤

**塊級標籤：**獨立占一行，不和其他元素待在同一行；能設置寬高

常見的標籤有 - `div, p, h1-h6, ul, li, ol, dl, dt, dd`

**行級標籤：**能和其他元素待在同一行；不能設置寬高

常見的標籤有 - `a, span, strong, em, u(underline)`

**行內塊元素：**能和其他元素待在一行；能設置寬高

常見的標籤有 - `img, input, textarea`

### html5中的transform 2D變換

2d移動是2d轉換裡面的一種功能，可以改變元素在頁面中的位置，類似定位。

**使用2d移動的步驟：**

- 1. 給元素添加轉換屬性`transform`
- 2. 屬性值為`tranlate`

demo:

```html
div{
  transfrom: translate(50px, 50px)
}
```

warning:

- 1. `translate`類似定位，不會影響到其他元素的位置
- 2. 對行內標籤沒有效果
- 3. `translate`中的百分比單位是相對於自身元素的`translare: (50%, 50%)`；可以和定位搭配使用，實現讓盒子居中佈局（用法和`margin`負值相同）

### html5中的語義化標籤

> 語義化標籤的好處是增強了代碼的可閱讀性，搜索引擎優化更好，對瀏覽器更友好

- `header`頭部
- `nav`導航
- `article` 文章，內容
- `aside` 側邊欄
- `section`塊
- `footer`頁腳