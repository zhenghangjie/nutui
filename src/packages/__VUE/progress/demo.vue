<template>
  <div class="demo full">
    <h2>{{ translate('basic') }}</h2>
    <div>
      <nut-cell>
        <nut-progress percentage="120" />
      </nut-cell>
    </div>

    <h2>{{ translate('customStyle') }}</h2>
    <div>
      <nut-cell>
        <nut-progress percentage="30" stroke-color=" rgba(250,44,25,0.47)" stroke-width="20" text-color="red" />
      </nut-cell>
    </div>
    <h2>{{ translate('noShowPercentage') }}</h2>
    <div>
      <nut-cell>
        <nut-progress percentage="50" :show-text="false" stroke-height="24" />
      </nut-cell>
    </div>
    <h2>{{ translate('showInsidePercentage') }}</h2>
    <div>
      <nut-cell>
        <nut-progress percentage="60" :text-inside="true" />
      </nut-cell>
    </div>
    <h2>{{ translate('customContent') }}</h2>
    <div>
      <nut-cell>
        <nut-progress percentage="60" :text-inside="true">
          <img
            src="https://img11.360buyimg.com/imagetools/jfs/t1/137646/13/7132/1648/5f4c748bE43da8ddd/a3f06d51dcae7b60.png"
            width="30"
            height="30"
            style="display: block"
          />
        </nut-progress>
      </nut-cell>
    </div>
    <h2>{{ translate('customSize') }}</h2>
    <div>
      <nut-cell>
        <nut-progress percentage="30" :text-inside="true" size="small"> </nut-progress>
      </nut-cell>
      <nut-cell>
        <nut-progress percentage="50" :text-inside="true" size="base"> </nut-progress>
      </nut-cell>
      <nut-cell>
        <nut-progress percentage="70" :text-inside="true" size="large"> </nut-progress>
      </nut-cell>
    </div>
    <h2>{{ translate('statusDisplay') }}</h2>
    <div>
      <nut-cell>
        <nut-progress
          percentage="30"
          stroke-color="linear-gradient(270deg, rgba(18,126,255,1) 0%,rgba(32,147,255,1) 32.815625%,rgba(13,242,204,1) 100%)"
          status="active"
        />
      </nut-cell>
      <nut-cell>
        <nut-progress percentage="50" status="icon" />
      </nut-cell>
      <nut-cell>
        <nut-progress
          percentage="100"
          stroke-color="linear-gradient(90deg, rgba(180,236,81,1) 0%,rgba(66,147,33,1) 100%)"
          stroke-width="15"
          status="icon"
        >
          <template #iconName>
            <Issue color="red" width="15px" height="15px"></Issue>
          </template>
        </nut-progress>
      </nut-cell>
    </div>
    <h2>{{ translate('dynamicChange') }}</h2>
    <div>
      <nut-cell>
        <nut-progress :percentage="val" />
      </nut-cell>
      <nut-cell>
        <nut-button type="default" @click="setReduceVal">{{ translate('reduce') }}</nut-button>
        <nut-button type="primary" @click="setAddVal">{{ translate('add') }}</nut-button>
      </nut-cell>
    </div>
  </div>
</template>

<script lang="ts">
import { ref } from 'vue';
import { createComponent } from '@/packages/utils/create';
const { createDemo, translate } = createComponent('progress');
import { useTranslate } from '@/sites/assets/util/useTranslate';
import { Issue } from '@nutui/icons-vue';
const initTranslate = () =>
  useTranslate({
    'zh-CN': {
      basic: '基础用法',
      customStyle: '设置颜色高度',
      noShowPercentage: '设置百分比不显示',
      showPercentage: '设置百分比外显',
      showInsidePercentage: '设置百分比内显',
      customContent: '设置百分比内显自定义内容',
      customSize: '自定义尺寸',
      statusDisplay: '设置状态显示',
      dynamicChange: '动态改变',
      reduce: '减少',
      add: '增加'
    },
    'en-US': {
      basic: 'Basic Usage',
      customStyle: 'Custom Style',
      noShowPercentage: 'Don’t Show Percentage',
      showPercentage: 'Percentage displayed outside',
      showInsidePercentage: 'Percentage displayed inside',
      customContent: 'Custom Content',
      customSize: 'Custom Size',
      statusDisplay: 'Status Display',
      dynamicChange: 'Dynamic Change',
      reduce: 'reduce',
      add: 'add'
    }
  });
export default createDemo({
  components: { Issue },
  props: {},
  setup() {
    initTranslate();
    const val = ref(0);
    const setAddVal = () => {
      if (val.value >= 100) {
        return false;
      }
      val.value += 10;
    };
    const setReduceVal = () => {
      if (val.value <= 0) {
        return false;
      }
      val.value -= 10;
    };
    return {
      val,
      setAddVal,
      setReduceVal,
      translate
    };
  }
});
</script>

<style lang="scss" scoped>
.nut-button {
  margin-right: 10px;
}
</style>
