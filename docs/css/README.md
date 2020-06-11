# CSS魔法

實際場景中使用的比较频繁的`CSS TRICK`。

場景的目錄如下：

- <a href="#/css/README?id=文本溢出隱藏">文本溢出隱藏</a>
- <a href="#/css/README?id=滾動條美化">滾動條美化</a>
- <a href="#/css/README?id=css等高限制">css等高限制</a>

## 文本溢出隱藏

**單行文本超出隱藏**

```css
{
  white-space: nowrap; // 超出區域內容不換行展示
  overflow: hidden; // 超出指定區域隱藏不可見
  text-overflow: ellipsis; // 超出的文本以省略號代替
}
```

**多行文本超出隱藏**

```css
{
  overflow: hidden;
  text-overflow: ellipsis;
  text-overflow: -o-ellipsis-lastline; // 對opera的兼容
  display: -webkit-box;
  -webkit-line-clamp: 2; // 行數
  -webkit-box-orient: vertical; // 設置為垂直方向
}
```

## 滾動條美化

對PC端項目的滾動條樣式進行改寫：

```less
/* x 和 y 軸自定義滾動，不限制寬高 */
.scroll_bar{
  height: 100%;
  overflow-y: auto;
  overflow-x: auto;
  &::-webkit-scrollbar {/*滾動條整體樣式*/
    width: 5px;
    height: 5px;
  }
  &::-webkit-scrollbar-thumb {/*滾動條裡面的方塊*/
    border-radius: 10px;
    -webkit-box-shadow: inset 0 0 5px rgba(0,0,0,0.2);
    background: #999;
    &:hover{
      background: #535353;
    }
  }
  &::-webkit-scrollbar-track {/*滾動條軌道*/
    -webkit-box-shadow: inset 0 0 5px rgba(0,0,0,0.2);
    border-radius: 10px;
    background: #EDEDED;
  }
}
```

## css等高限制

同級的`div`因為不同的高度會造成排版的參差不齊。可以通過下面的樣式來解決。

```vue
<template>
  <div>
    <a-row v-if="return_mode_detail.info" style="margin-top: 30px; display: flex; flex-wrap: wrap;">
      <a-col :xs="24" :md="12" :lg="8" v-for="(item, index) in JSON.parse(return_mode_detail.info)" :key="index">
        <div class="return_mode_item">
          <a-avatar :style="{ backgroundColor: 'red', verticalAlign: 'middle' }" class="serial">{{index+1}}</a-avatar>
          <div class="title">{{item.title}}</div>
          <div class="intro">{{item.intro}}</div>
        </div>
      </a-col>
    </a-row>
  </div>
</template>
```

重點的樣式如下：

```css
.parent{
  display: flex;
  flex-wrap: wrap;
}
```