# CSS魔法

實際場景中使用的比较频繁的`CSS TRICK`。

場景的目錄如下：

- <a href="#/css/README?id=文本溢出隱藏">文本溢出隱藏</a>
- 

## 文本溢出隱藏

**當行文本超出隱藏**

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