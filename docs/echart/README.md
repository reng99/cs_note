# ECHART魔法

- <a href="#/echart/README?id=圖表自適應">圖表自適應</a>

## 圖表自適應

在瀏覽器窗口發生改變時候，調用`echart`的`resize`方法。

結合`vue-ant-design`來展示下：

```vue
<template>
  <div ref="container">
    <a-row>
      <a-col :sm="24" :md="12">
        <a-spin :spinning="loading" tip="loading...">
          <div ref="asset_resource" :style="{width: '100%', minHeight: '360px', height: three_col_width+'px'}" />
        </a-spin>
      </a-col>
    </a-row>
  </div>
</template>

<script>
export default {
  name: "demo",
  data() {
    container_width: 0, // 容器宽度
    three_col_width: 0, // 三列的各列的宽
  },
  mounted() {
    let vm = this
    vm.$nextTick(() =>{
      vm.getContainerWidth()
      vm.drawLine()
      window.onresize = () => {
        vm.getContainerWidth()
        // 使用echart的方法重新繪製圖表
        const chartAR = echarts.init(vm.$refs['asset_resource']) // 這裡默認已經全局引入了echart
        chartAR.resize() // resize 是重點
      }
    })
  },
  methods: {
    // 繪製圖表
    drawLine() {
      let vm = this
      const chartAR = echarts.init(vm.$refs['asset_resource'])
      // 具體的實現邏輯
      // ...
    },
    // 获取容器的宽度
    getContainerWidth() {
      let vm = this
      this.$nextTick(function() {
        vm.container_width = vm.$refs.container.clientWidth
        vm.three_col_width = Math.floor((vm.container_width - 15) / 3)
      })
    },
  }
}
</script>
```