<template>
  <t-card :bordered="false">
    <t-form
      ref="form"
      :data="formData"
      :rules="FORM_RULES"
      label-align="top"
      :label-width="100"
      @reset="onReset"
      @submit="onSubmit"
    >
      <div class="form-basic-container">
        <div class="form-basic-item">
          <div class="form-basic-container-title">用例信息</div>
          <!-- 表单内容 -->

          <!-- 用例名称,用例类型 -->
          <t-row class="row-gap" :gutter="[16, 24]">
            <t-col :span="12">
              <t-form-item label="用例名称" name="name">
                <t-input v-model="formData.name" placeholder="请输入内容" />
              </t-form-item>
            </t-col>

            <t-col :span="9">
              <t-form-item label="用例分类" name="category">
                <t-select
                  v-model="formData.category"
                  placeholder="请选择用例分类"
                  class="demo-select-base"
                  clearable
                >
                  <t-option v-for="(item, index) in categoryOptions" :key="index" :value="item.value" :label="item.label">
                    {{ item.label }}
                  </t-option>
                </t-select>
              </t-form-item>
            </t-col>
            <t-col :span="3">
              <t-form-item label="优先级" name="priority">
                <t-select
                  v-model="formData.priority"
                  placeholder="请选择优先级"
                  class="demo-select-base"
                  clearable
                >
                  <t-option v-for="(item, index) in priorityOptions" :key="index" :value="item.value" :label="item.label">
                    {{ item.label }}
                  </t-option>
                </t-select>
              </t-form-item>
            </t-col>
            <t-col :span="12">
              <t-form-item label="用例描述" name="comment">
                <t-textarea v-model="formData.description" :height="124" placeholder="请输入备注" />
              </t-form-item>
            </t-col>

          </t-row>


          <div class="form-basic-container-title form-title-gap">其它信息</div>


        </div>
      </div>

      <div class="form-submit-container">
        <div class="form-submit-sub">
          <div class="form-submit-left">
            <t-button theme="primary" class="form-submit-confirm" type="submit"> 创建 </t-button>
            <t-button type="reset" class="form-submit-cancel" theme="default" variant="base"> 取消 </t-button>
          </div>
        </div>
      </div>
    </t-form>
  </t-card>
</template>
<script>
import { prefix } from '@/config/global';

const INITIAL_DATA = {
  name: '',
  description: '',
  // category: 1,
  priority: 1,
  // create_user: 1,
  // update_user: 1,
};
const FORM_RULES = {
  name: [{ required: true, message: '请输入用例名称', type: 'error' }],
  type: [{ required: true, message: '请选择用例类型', type: 'error' }],
  payment: [{ required: true, message: '请选择用例收付类型', type: 'error' }],
  amount: [{ required: true, message: '请输入用例金额', type: 'error' }],
  partyA: [{ required: true, message: '请选择甲方', type: 'error' }],
  partyB: [{ required: true, message: '请选择乙方', type: 'error' }],
  signDate: [{ required: true, message: '请选择日期', type: 'error' }],
  startDate: [{ required: true, message: '请选择日期', type: 'error' }],
  endDate: [{ required: true, message: '请选择日期', type: 'error' }],
};

export default {
  name: 'FormBase',
  data() {
    return {
      prefix,
      stepSuccess: true,
      formData: { ...INITIAL_DATA },
      FORM_RULES,
      priorityOptions: [
        { label: 'P0', value: '0' },
        { label: 'P1', value: '1' },
        { label: 'P2', value: '2' },
      ],
      categoryOptions: [
        { label: '分类1', value: '1' },
        { label: '分类2', value: '2' },
        { label: '分类3', value: '3' },
      ],

      textareaValue: '',
      rules: {
        name: [{ required: true, message: '请输入用例名称', type: 'error' }],
        type: [{ required: true, message: '请选择用例类型', type: 'error' }],
        payment: [{ required: true, message: '请选择用例收付类型', type: 'error' }],
        amount: [{ required: true, message: '请输入用例金额', type: 'error' }],
        partyA: [{ required: true, message: '请选择甲方', type: 'error' }],
        partyB: [{ required: true, message: '请选择乙方', type: 'error' }],
        signDate: [{ required: true, message: '请选择日期', type: 'error' }],
        startDate: [{ required: true, message: '请选择日期', type: 'error' }],
        endDate: [{ required: true, message: '请选择日期', type: 'error' }],
      },
    };
  },
  methods: {
    onReset() {
      this.$message.warning('取消新建');
    },
    onSubmit({ validateResult }) {
      if (validateResult === true) {
        this.$request
          .post(`/api/testcases/`, this.formData)
          .then(() => {
            this.$message.success('新建成功');
          })
          .catch((e) => {
            this.$message.error('新建失败');
            console.log(e);
          })
          .finally(() => {
            this.loadData();
          });

      }
    },
  },
};
</script>
<style lang="less" scoped>
@import './index';
</style>
