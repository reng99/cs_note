# 數據結構

![banner](./imgs/banner.jpg "border_img_banner")

隨著應用程序越來越複雜和數據越來越豐富，有三個問題需要應對：

- **數據搜索** - 考慮到倉庫裡面有一百萬的庫存的項目數據。如果應用程序在搜索一條項目數據的時候，它每次要在一百萬的數據裡面查找一條數據，這會降低搜索的速度。伴隨著數據的增長，搜索會更慢。

- **處理器的速度** - 雖然處理器的運行速度總是非常快的，但是數據增長到以億計的時候，將會因為限制而降低速度。

- **多條請求** - 數千的用戶可以同時在一台`web`服務器上面搜索數據。在搜索數據的時候，即使是性能很好的服務器也會失敗。

為了解決上面提出的問題，`數據結構`應勢而生。數據被很好的組織在`數據結構`裡，在那裡，請求的數據可以很快的被搜索到。

那麼~

**數據結構**是計算機存儲、組織數據的方式。

**數據結構**是指相互之間存在一種或者多種特殊關係的數據元素的集合。

通常情況下，精心選擇`數據結構`可以帶來更高的運行或者存儲效率。

`數據結構`可以分為`線性結構`和`非線性結構`。

1. **線性結構**是一個有序數據元素的集合。它應該滿足下面的特征：

- 集合中必存在唯一的一個`第一個元素`
- 集合中必存在唯一的一個`最後的元素`
- 除最後一個元素之外，其他數據元素均有唯一的`後繼`
- 除第一個元素之外，其他數據元素均有唯一的`前驅`

按照百度百科的定義，我們知道符合條件的數據結構有棧、隊列等。

2. **非線性結構**其邏輯特征是一個節點元素可以有多個直接前驅或多個直接後繼。

那麼符合條件的數據結構就有圖、樹等。

嗯~有這麼個概念就行了，下面我們來了解下**數據結構**。

> :warning: 此處的**集合**跟我們即將要了解的**數據結構-集合**是不同的