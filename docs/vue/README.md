# VUE魔法

實用的`VUE TRICK`。

目錄如下：

- <a href="#/vue/README?id=slot插槽">slot插槽</a>
- <a href="#/vue/README?id=watch監聽">watch監聽</a>
- <a href="#/vue/README?id=獲取div的高度">獲取div的高度</a>
- <a href="#/vue/README?id=判斷內容是否溢出">判斷內容是否溢出</a>
- <a href="#/vue/README?id=阻止事件冒泡">阻止事件冒泡</a>
- <a href="#/vue/README?id=directive指令">directive指令</a>
- <a href="#/vue/README?id=delimiters分割符">delimiters分割符</a>

## slot插槽

我們在使用諸如[vue ant design](https://www.antdv.com/docs/vue/introduce-cn/)、[vant](https://youzan.github.io/vant/#/zh-CN/)等框架進行開發的時候，會更改其封裝好的組件的內容。那麼，就使用到[slot](https://cn.vuejs.org/v2/guide/components-slots.html)進行更改。

那麼，我們開發自己組件的時候，也可以使用`slot`來更好的管理組件，達到複用的效果。下面是實際中最常用到的：

```vue
<template>
  <div>
    <slot name="title">default title</slot>
    component
  </div>
</template>
```

```vue
<template>
  <div>
    <slot-component>
      <h3 slot="title">title</h3>
    </slot-component>
    main
  </div>
</template>

<script>
import SlotComponent from "/path/to/slot-component-above"
export default {
  name: "main",
  data() {
    return {}
  },
  components: {
    SlotComponent
  }
}
</script>
```

## watch監聽

當值發生變動的時候，你可以通過`watch`對數據進行處理。

```vue
<template>
  <div>
    <input v-model="data" />
  </div>
</template>

<script>
export default {
  name: 'watch',
  data() {
    return {
      data: ""
    }
  },
  watch: {
    data: {
      handler: function(newVal, oldVal) {
        console.log('new val -> ', newVal, ' old val -> ', oldVal)
      },
      deep: true
    }
  },
}
</script>
```

## 獲取div的高度

`vue`中需要獲取元素的高度，我們可以通過下面的方式獲取。

```vue
<template>
  <a-row>
    <a-col :md="6">
      <a-row class="catalogue scrollbar" :style="{maxHeight: `${treeHeight}px`}">
        <!--something here-->
      </a-row>
    </a-col>
    <a-col :md="18">
      <a-row ref="mainContent">
        <!--something here-->
      </a-row>
    </a-col>
  </a-row>
</template>

<script>
export default {
  name: 'demo',
  data() {
    treeHeight: 600,
  },
  methods: {
    // 看情況獲取相關方法中對應元素的高度
  },
  mounted() {
    let vm = this
    vm.$nextTick(() => {
      const height = vm.$refs.mainContent.$el.clientHeight
      if(height && height > 600) {
        vm.treeHeight = height
      } else{
        vm.treeHeight = 600
      }
    })
  }
}
</script>
```

## 判斷內容是否溢出

溢出的判斷，是比較滾動的寬度和容器的寬度進行對比，也就是：

```javascript
el.clientWidth < el.scrollWidth
```

具體的實現我封裝成了一個組件，如下：

```javascript
<template>
  <div class="ellipsis_comp">
    <a-tooltip :title="isEllipsis ? data.slice(data.indexOf('</b>')+4) : ''">
      <p :class="[isEllipsis ? 'ellipsis' : '']" ref="ellipsis" v-html="data" />
    </a-tooltip>
  </div>
</template>

<script>
export default {
  name: 'ellipsis',
  data() {
    return {
      isEllipsis: false
    }
  },
  props: {
    data: String
  },
  methods: {},
  mounted() {
    this.$nextTick(() => {
      const el = this.$refs.ellipsis
      this.isEllipsis = el.clientWidth < el.scrollWidth
    })
  }
}
</script>

<style lang="less" scoped>
.ellipsis_comp{
  .ellipsis{
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
}
</style>
```

## 阻止事件冒泡

這裡的阻止事件冒泡並非原生的，是`vue`提供的，感興趣可以看看`preventDefault`、`stopPropagation`和`return false`等。

- stop 阻止冒泡事件，對標`stopPropagation`

- prevent 阻止系統的默認事件，對標`preventDefault`

- once 只添加一次事件

```vue
<template>
  <div @click.prevent.stop.once="method">
    content
  </div>
</template>
```

## directive指令

`directive`指令可以全局載入，也可以局部組件載入：

> 詳情見[官網](https://cn.vuejs.org/v2/guide/custom-directive.html)

### 全局載入

比如：

```javascript
// 註冊一個全局自定義指令 'v-focus'
Vue.directive('focus', {
  // 當被綁定的元素插入到 DOM 中時...
  inserted: function(el) {
    // 聚焦元素
    el.focus()
  }
})
```

```html
<input v-focus />
```

### 局部載入

比如：

```javascript
directives: {
  focus: {
    // 指令的定義
    inserted: function(el) {
      el.focus()
    }
  }
}
```

```html
<input v-focus />
```

### 鉤子函數

一個指令自定義對象可以提供如下的幾個鉤子函數（均為可選）：

- `bind`: 只調用一次，指令第一次綁定到元素時調用。這裡可以進行一次性的初始化設置。

- `inserted`：被綁定元素插入父節點時調用（僅保證父節點存在，但不一定已被插入文檔中）。

- `update`：所有組件的VNode更新時調用，**但是可能發生在其子VNode更新之前**。

- `componentUpdated`：指令所在的組件的VNode**及其子**VNode全部更新後調用。

- `unbind`：只調用一次，指令和其元素解綁時調用

## delimiters分割符

`VUE`默認是`{{}}`的分隔符，即默認值為`["{{", "}}"]`。可以進行修改，如下：

```javascript
new Vue({
  delimiters: ["${", "}"]
})
```
不過修改這種既定的分隔符意義不大~