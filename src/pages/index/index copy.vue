<template>
  <view class="index">
    {{ msg }} 
    <view class="btn">
      <nut-button type="primary" @click="handleClick('text', msg2, true)">点我</nut-button>
    </view>
     <view class="form-list">
       <Input placeholder="请输入文本" />
       <Form>
        <Form.Item v-for="(item, index) in fromList" :key="index"
          label={{item.label}}
          name={{item.label}}
        >
          <Input placeholder="请输入姓名" type="text" />
        </Form.Item>
      </Form>
    </view>
    <nut-toast :msg="msg2" v-model:visible="show" :type="type" :cover="cover"/>
  </view>
</template>

<script>
import { reactive, toRefs } from 'vue';
import { Dongdong } from '@nutui/icons-vue-taro'
import { Form, Input, Picker } from '@nutui/nutui-react';

import { fromList } from '../../mock/data';
console.log("fromList", fromList); 

export default {
  name: 'Index',
  components: {
  },
  setup() {
    const state = reactive({
      msg: '欢迎使用 NutUI4.0 开发小程序',
      msg2: '你成功了～',
      type: 'text',
      show: false,
      cover: false,
      fromList: fromList
    });

    const handleClick = (type, msg, cover = false) => {
      state.show = true;
      state.msg2 = msg;
      state.type = type;
      state.cover = cover;
    };

    return {
      ...toRefs(state),
      handleClick
    }
  }
}
</script>

<style lang="scss">
.index {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
</style>
