# Cascader

### Intro

The cascader component is used for the selection of multi-level data. The typical scene is the selection of provinces and cities.

### Install

```js
import { createApp } from 'vue';
import { Cascader } from '@nutui/nutui';

const app = createApp();
app.use(Cascader);
```

### Basic Usage

Pass in the `options` list.
:::demo
```html
<template>
  <nut-cell title="select address" :desc="demo1.value.toString() || 'please select address'" @click="demo1.visible = true"> </nut-cell>
  <nut-cascader
    title="select address"
    v-model:visible="demo1.visible"
    v-model="demo1.value"
    @change="events.change"
    @pathChange="events.pathChange"
    :options="demo1.options"
  ></nut-cascader>
</template>
<script lang="ts">
import { reactive } from 'vue';
export default {
setup() {
  // 基础用法
  const demo1 = reactive({
    visible: false,
    value: ['湖南'],
    options: [
      {
        value: '浙江',
        text: '浙江',
        children: [
          {
            value: '杭州',
            text: '杭州',
            disabled: true,
            children: [
              { value: '西湖区', text: '西湖区' },
              { value: '余杭区', text: '余杭区' }
            ]
          },
          {
            value: '温州',
            text: '温州',
            children: [
              { value: '鹿城区', text: '鹿城区' },
              { value: '瓯海区', text: '瓯海区' }
            ]
          }
        ]
      },
      {
        value: '湖南',
        text: '湖南',
        disabled: true
      },
      {
        value: '福建',
        text: '福建',
        children: [
          {
            value: '福州',
            text: '福州',
            children: [
              { value: '鼓楼区', text: '鼓楼区' },
              { value: '台江区', text: '台江区' }
            ]
          }
        ]
      }
    ]
  });
  const events = {
    change(...args: any) {
      console.log('change', ...args);
    },
    pathChange(...args: any) {
      console.log('pathChange', ...args);
    }
  };

  return {
    demo1,
    events
  }
}
}
</script>
```
:::

### Custom attribute name

use `textKey`、`valueKey`、`childrenKey`Specify the property name.
:::demo
```html
<template>
    <nut-cell title="select address" :desc="demo2.value.toString() || 'please select address'" @click="demo2.visible = true"> </nut-cell>
    <nut-cascader
      title="select address"
      v-model:visible="demo2.visible"
      v-model="demo2.value"
      labelKey="text"
      @change="events.change"
      @pathChange="events.pathChange"
      valueKey="text"
      childrenKey="items"
      :options="demo2.options"
    ></nut-cascader>
</template>
<script lang="ts">
import { reactive } from 'vue';
export default {
  setup() {
    // 自定义属性名称
    const demo2 = reactive({
      visible: false,
      value: ['福建', '福州', '台江区'],
      options: [
        {
          text: '浙江',
          items: [
            {
              text: '杭州',
              disabled: true,
              items: [{ text: '西湖区' }, { text: '余杭区' }]
            },
            {
              text: '温州',
              items: [{ text: '鹿城区' }, { text: '瓯海区' }]
            }
          ]
        },
        {
          text: '福建',
          items: [
            {
              text: '福州',
              items: [{ text: '鼓楼区' }, { text: '台江区' }]
            }
          ]
        }
      ]
    });
    const events = {
      change(...args: any) {
        console.log('change', ...args);
      },
      pathChange(...args: any) {
        console.log('pathChange', ...args);
      },
    };

    return { demo2, events };
  }
}
</script>
```
:::

### Async loading

Use `lazy` to identify whether data needs to be obtained dynamically. At this time, not transmitting `options` means that all data needs to be loaded through `lazyload` . The first loading is distinguished by the `root` attribute. When a non leaf node is encountered, the `lazyload` method will be called. The parameters are the current node and the `resolve` method. Note that the `resolve` method must be called. If it is not transmitted to a child node, it will be treated as a leaf node.
:::demo
```html
<template>
    <nut-cell title="select address" :desc="demo3.value.toString() || 'please select address'" @click="demo3.visible = true"> </nut-cell>
    <nut-cascader
      title="select address"
      v-model:visible="demo3.visible"
      v-model="demo3.value"
      @change="events.change"
      @pathChange="events.pathChange"
      lazy
      :lazyLoad="demo3.lazyLoad"
    ></nut-cascader>
</template>
<script lang="ts">
import { reactive } from 'vue';
export default {
  setup() {
    const demo3 = reactive({
      visible: false,
      value: ['A0', 'A12', 'A23', 'A32'],
      lazyLoad(node: any, resolve: (children: any) => void) {
        setTimeout(() => {
          // root表示第一层数据
          if (node.root) {
            resolve([
              { value: 'A0', text: 'A0' },
              { value: 'B0', text: 'B0' },
              { value: 'C0', text: 'C0' }
            ]);
          } else {
            const { value, level } = node;
            const text = value.substring(0, 1);
            const value1 = `${text}${level + 1}1`;
            const value2 = `${text}${level + 1}2`;
            const value3 = `${text}${level + 1}3`;
            resolve([
              { value: value1, text: value1, leaf: level >= 6 },
              { value: value2, text: value2, leaf: level >= 6 },
              { value: value3, text: value3, leaf: level >= 6 }
            ]);
          }
        }, 3000);
      }
    });
    const events = {
      change(...args: any) {
        console.log('change', ...args);
      },
      pathChange(...args: any) {
        console.log('pathChange', ...args);
      },
    };

    return { demo3, events };
  }
}
</script>
```
:::

### Async loading of partial data

:::demo
```html
<template>
  <nut-cell title="select address" :desc="demo4.value.toString() || 'please select address'" @click="demo4.visible = true"> </nut-cell>
  <nut-cascader
    title="select address"
    v-model:visible="demo4.visible"
    v-model="demo4.value"
    @change="events.change"
    @pathChange="events.pathChange"
    :options="demo4.options"
    lazy
    :lazyLoad="demo4.lazyLoad"
  ></nut-cascader>
</template>
<script lang="ts">
import { reactive } from 'vue';
export default {
  setup() {
    const demo4 = reactive({
      visible: false,
      value: [],
      options: [
        { value: 'A0', text: 'A0' },
        {
          value: 'B0',
          text: 'B0',
          children: [
            { value: 'B11', text: 'B11', leaf: true },
            { value: 'B12', text: 'B12' }
          ]
        },
        { value: 'C0', text: 'C0' }
      ],
      lazyLoad(node: any, resolve: (children: any) => void) {
        setTimeout(() => {
          const { value, level } = node;
          const text = value.substring(0, 1);
          const value1 = `${text}${level + 1}1`;
          const value2 = `${text}${level + 1}2`;
          resolve([
            { value: value1, text: value1, leaf: level >= 2 },
            { value: value2, text: value2, leaf: level >= 1 }
          ]);
        }, 500);
      }
    });
    const events = {
      change(...args: any) {
        console.log('change', ...args);
      },
      pathChange(...args: any) {
        console.log('pathChange', ...args);
      },
    };

    return { demo4, events };
  }
}
</script>
```
:::

### Automatic data conversion

If your data is a flat structure that can be converted into a tree structure, you can tell the component that automatic conversion is required through `convertConfig`, ` convertConfig` accepts four parameters, `topid` is the parent ID of the top-level node, `idkey` is the unique ID of the node, `pidkey` is the attribute name pointing to the parent node ID, and if there is a `sortkey`, `Array.prototype.sort()` to sort at the same level.


:::demo
```html
<template>
    <nut-cell title="select address" :desc="demo5.value.toString() || 'please select address'" @click="demo5.visible = true"> </nut-cell>
    <nut-cascader
      title="select address"
      v-model:visible="demo5.visible"
      v-model="demo5.value"
      @change="events.change"
      @pathChange="events.pathChange"
      :options="demo5.options"
      :convertConfig="demo5.convertConfig"
    ></nut-cascader>
</template>
<script lang="ts">
import { reactive } from 'vue';
export default {
  setup() {
    const demo5 = reactive({
      visible: false,
      value: ['广东省', '广州市'],
      convertConfig: {
        topId: null,
        idKey: 'id',
        pidKey: 'pid',
        sortKey: ''
      },
      options: [
        { value: '北京', text: '北京', id: 1, pid: null },
        { value: '朝阳区', text: '朝阳区', id: 11, pid: 1 },
        { value: '亦庄', text: '亦庄', id: 111, pid: 11 },
        { value: '广东省', text: '广东省', id: 2, pid: null },
        { value: '广州市', text: '广州市', id: 21, pid: 2 }
      ]
    });
    const events = {
      change(...args: any) {
        console.log('change', ...args);
      },
      pathChange(...args: any) {
        console.log('pathChange', ...args);
      },
    };

    return { demo5, events };
  }
}
</script>
```
:::

## API

### Props

| Attribute           | Description                                                                                                  | Type     | Default     |
|---------------------|--------------------------------------------------------------------------------------------------------------|----------|-------------|
| v-model             | Selected value, bidirectional binding                                                                        | Array    | -           |
| v-model:visible             | selected layer                                      | boolean    | `false`           |
| options             | Cascade data                                                                                                 | Array    | -           |
| lazy                | Whether to enable dynamic loading                                                                            | boolean  | -           |
| lazy-load           | Dynamic loading callback, which takes effect when dynamic loading is enabled                                 | Function | -           |
| value-key           | Customize the field of `value` in the `options` structure                                                    | string   | -           |
| text-key            | Customize the fields of `text` in the `options` structure                                                    | string   | -           |
| children-key        | Customize the fields of `children` in the `options` structure                                                | string   | -           |
| convert-config      | When options is a flat structure that can be converted into a tree structure, configure the conversion rules | object   | -           |
| title               | Title                                                                                                        | string   | `''`          |
| close-icon-position | Cancel the button position and inherit the popup component                                                   | string   | `"top-right"` |
| closeable           | Whether to display the close button and inherit the popup component                                          | boolean  | `true`        |
| poppable            | Whether to display the popup（After setting to false, the title is invalid）                                 | boolean  | `true`        |

### Events

| Event       | Description                               | Arguments          |
|-------------|-------------------------------------------|--------------------|
| change      | Triggered when the selected value changes | `(value, pathNodes)` |
| path-change | Triggered when the selected item changes  | `(pathNodes) `       |

## Theming

### CSS Variables

The component provides the following CSS variables, which can be used to customize styles. Please refer to [ConfigProvider component](#/en-US/component/configprovider).

| Name                             | Default Value              |
|----------------------------------|----------------------------|
| --nut-cascader-font-size         | _var(--nut-font-size-2)_   |
| --nut-cascader-line-height       | _22px_                     |
| --nut-cascader-tabs-item-padding | _0 10px_                   |
| --nut-cascader-bar-padding       | _24px 20px 17px_           |
| --nut-cascader-bar-font-size     | _var(--nut-font-size-4)_   |
| --nut-cascader-bar-line-height   | _20px_                     |
| --nut-cascader-bar-color         | _var(--nut-title-color)_   |
| --nut-cascader-item-padding      | _10px 20px_                |
| --nut-cascader-item-color        | _var(--nut-title-color)_   |
| --nut-cascader-item-font-size    | _var(--nut-font-size-2)_   |
| --nut-cascader-item-active-color | _var(--nut-primary-color)_ |
