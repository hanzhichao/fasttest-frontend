<template>
  <div class="detail-base">
    <t-card title="基本信息" :bordered="false" class="info-block">
      <t-descriptions>
        <t-descriptions-item v-for="(item, index) in baseInfoData" :key="index" :label="item.name">
          <span
            slot="content"
            :class="{
              ['inProgress']: item.type && item.type.value === 'inProgress',
              ['pdf']: item.type && item.type.value === 'pdf',
            }"
          >
            <i v-if="item.type && item.type.key === 'contractStatus'" />
            {{ item.value }}
          </span>
        </t-descriptions-item>
      </t-descriptions>
    </t-card>

    <t-card title="变更记录" class="container-base-margin-top" :bordered="false">
      <t-steps class="detail-base-info-steps" layout="vertical" theme="dot" :current="1">
        <t-step-item title="上传合同附件" content="这里是提示文字" />
        <t-step-item title="修改合同金额" content="这里是提示文字" />
        <t-step-item title="新建合同" content="2020-12-01 15:00:00 管理员-李川操作" />
      </t-steps>
    </t-card>
  </div>
</template>
<script>
import { prefix } from '@/config/global';
import model from '@/service/service-detail-base';

export default {
  name: 'DetailBase',
  data() {
    return {
      prefix,
      baseInfoData: model.getBaseInfoData(),
    };
  },
  methods: {},
};
</script>
<style lang="less" scoped>
@import './index';
</style>
