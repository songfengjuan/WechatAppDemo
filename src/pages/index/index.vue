<template>
  <view class="demo-body">
    <nut-form class="demo-form-body" :model="form" :label-width="100">
      <nut-form-item
        v-for="(field, index) in fields"
        :key="index"
        :label="field.label"
        :prop="field.prop"
        :rules="field.rules"
        class="demo-form-item"
      >
        <nut-input
          v-if="field.type !== pickerType"
          v-model="form[field.prop]"
          :type="field.type"
          placeholder=""
        />

        <nut-cell
          v-if="field.type === pickerType"
          title="select"
          v-model="form[field.prop]"
          :desc="popupListValue[index]"
          @click="() => handleShow(index)"
        />
      </nut-form-item>
      <nut-popup
        v-if="popupIndex > -1"
        position="bottom"
        v-model:visible="show"
      >
        <nut-picker
          v-model="fields[popupIndex].value"
          :columns="columns"
          title="select"
          @confirm="popupConfirm"
          @cancel="show = false"
        >
        </nut-picker>
      </nut-popup>
      <view class="demo-submit-body">
        <nut-button type="success" @click="submitForm">提交</nut-button>
      </view>
    </nut-form>
  </view>
</template>
<script lang="ts">
import { ref, reactive, onMounted } from "vue";
import { fromList } from "../../mock/data";

interface FormField {
  label: string;
  prop: string;
  type: string;
  value: string;
  placeholder?: string;
  maxlength?: number;
  disabled?: boolean;
  readonly?: boolean;
  options?: any[];
  show?: boolean;
}

interface FormData {
  [key: string]: any;
}

interface optionsData {
  text?: string;
  value?: string;
}

const initTypeData = ["number", "digit", "password"];
const pickerType = "singleSelect";

export default {
  setup() {
    const show = ref(false);
    const popupListValue = ref<string[]>([]);
    const popupIndex = ref(-1);
    const columns = ref<any[]>([]);
    const form = ref<FormData>({
      // 表单数据
    });

    const fields = ref<FormField[]>([
      // 初始表单字段数组
    ]);

    onMounted(async () => {
      // 获取接口数据并设置表单字段
      // const response = await axios.get('/api/form-fields');
      const list = fromList as FormField[];
      list.forEach((item) => {
        if (item.options) {
          let newOptions: optionsData[] = [];
          item.options.map((child) => {
            let options: optionsData = {};
            options = {
              text: child,
              value: child,
            };
            newOptions.push(options);
          });
          item.options = newOptions;
          item.show = false;
        }
        if (!(initTypeData.includes(item.type) || item.type === pickerType)) {
          item.type = "text";
        }
      });
      fields.value = [...list];
      list.forEach((field) => {
        form.value[field.prop] = "";
      });
    });

    const submitForm = () => {
      // 提交表单
      console.log("form data:", form.value);
    };

    const popupConfirm = ({ selectedValue, selectedOptions }) => {
      const value: string = selectedOptions
        .map((val: any) => val.text)
        .join(",");
      popupListValue.value[popupIndex.value] = value;
      fields.value[popupIndex.value].value = value;
      form.value[fields.value[popupIndex.value].prop] = value;
      show.value = false;
    };

    const handleShow = (index: number) => {
      popupIndex.value = index;
      show.value = true;
      columns.value = fields.value[index].options || [];
    };

    return {
      form,
      fields,
      pickerType,
      popupIndex,
      show,
      columns,
      popupListValue,
      submitForm,
      handleShow,
      popupConfirm,
    };
  },
};
</script>

<style lang="scss">
.demo-body {
  padding: 0 20px;
  background: #ccc;
  height: 100vh;
}

.demo-form-item {
  display: flex;
  flex-direction: column;
  width: 100%;
  background: #fff;
  margin-bottom: 20rpx !important;
  border-radius: 0px !important;
}

.demo-submit-body {
  display: flex;
  width: 100%;
  justify-content: center;
}
</style>
