# VUE魔法

實用的`VUE TRICK`。

目錄如下：

- <a href="#/vue/README?id=slot插槽">slot插槽</a>
- <a href="#/vue/README?id=watch監聽">watch監聽</a>
- <a href="#/vue/README?id=獲取div的高度">獲取div的高度</a>

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