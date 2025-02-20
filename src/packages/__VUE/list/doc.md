# List 虚拟列表

### 介绍
在正常的列表展示以及上拉加载中，我们通常使用 `NutUI` 提供的 [滚动加载](#/zh-CN/infiniteloading) 组件，那如果我们加载的数据量非常大时，则可能会产生严重的性能问题，导致视图无法响应操作一段时间，这时候我们就用到了虚拟列表组件 `List`，它可以保证只渲染当前可视区域，其他部分在用户滚动到可视区域内之后再渲染。保证了页面流程度，提升性能。

### 安装

```javascript

import { createApp } from 'vue';
import { List } from '@nutui/nutui';

const app = createApp();
app.use();
```

### 基础用法

:::demo

```html
<template>
  <div class="demo">
    <h2>基础用法</h2>
    <nut-cell>
      <nut-list :height="50" :listData="count" @scroll-bottom="handleScroll">
        <template v-slot="{ item }">
          <div class="list-item">
            {{ item }}
          </div>
        </template>
      </nut-list>
    </nut-cell>
  </div>
</template>
<script lang="ts">
import { onMounted, reactive, toRefs } from 'vue';
export default {
  props: {},
  setup() {
    const state = reactive({
      count: new Array(100).fill(0)
    });

    const handleScroll = () => {
      let arr = new Array(100).fill(0);
      const len = state.count.length;
      state.count = state.count.concat(arr.map((item: number, index: number) => len + index + 1));
    };

    onMounted(() => {
      state.count = state.count.map((item: number, index: number) => index + 1);
    })

    return { ...toRefs(state), handleScroll };
  }
};
</script>
<style lang="scss">
body {
  width: 100%;
  height: 100vh;
}
#app {
  width: 100%;
  height: 100%;
}
.demo {
  height: 100%;
  .nut-cell {
    height: 100%;
  }
  .list-item {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 100%;
    background-color: #f4a8b6;
    border-radius: 10px;
  }
}
</style>
```

:::

## API

### Props

| 参数         | 说明                             | 类型   | 默认值           |
|--------------|----------------------------------|--------|------------------|
| height         | 列表项的高度               | number | `50`                |
| list-data         | 列表数据               | any[] | `[]`                |
| container-height        | 容器高度(最大值不能超过可视区)              | number | `可视区高度`                |

### Slots

| 名称         | 说明                             | 类型   |
|--------------|----------------------------------|--------|
| item         | 列表项数据               | object |
| index         | 索引               | number |

### Events

| 事件名 | 说明           | 回调参数     |
|--------|----------------|--------------|
| scroll-bottom   | 滚动到底部时触发 | - |


## 主题定制

### 样式变量

组件提供了下列 CSS 变量，可用于自定义样式，使用方法请参考 [ConfigProvider 组件](#/zh-CN/component/configprovider)。

| 名称                                    | 默认值                     |
| --------------------------------------- | -------------------------- |
| --nut-list-item-margin       | _0 0 10px 0_        |