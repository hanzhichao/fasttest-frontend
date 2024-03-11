<template>
  <div class="detail-base">
    <t-card title="基本信息" :bordered="false" class="info-block">
      <t-descriptions :data="data">
        <t-descriptions-item key="name" label="用例名称">
          <span slot="content" class="">
            {{ data.name }}
          </span>
        </t-descriptions-item>
        <t-descriptions-item key="description" label="用例描述">
          <span slot="content" class="">
            {{ data.description }}
          </span>
        </t-descriptions-item>
        <t-descriptions-item key="category" label="用例分类">
          <span slot="content" class="">
            {{ data.category }}
          </span>
        </t-descriptions-item>
        <t-descriptions-item key="priority" label="优先级">
          <span slot="content" class="">
            {{ data.priority }}
          </span>
        </t-descriptions-item>
        <!--        <t-descriptions-item key="description" label="用例标签">-->
        <!--          <span slot="content" class="" >-->
        <!--            {{ data.priority }}-->
        <!--          </span>-->
        <!--        </t-descriptions-item>-->
        <t-descriptions-item key="create_user" label="创建人">
          <span slot="content" class="">
            {{ data.create_user }}
          </span>
        </t-descriptions-item>
        <t-descriptions-item key="update_user" label="更新人">
          <span slot="content" class="">
            {{ data.update_user }}
          </span>
        </t-descriptions-item>
        <t-descriptions-item key="create_time" label="创建时间">
          <span slot="content" class="">
            {{ data.create_time }}
          </span>
        </t-descriptions-item>
        <t-descriptions-item key="update_time" label="更新时间">
          <span slot="content" class="">
            {{ data.update_time }}
          </span>
        </t-descriptions-item>
      </t-descriptions>
    </t-card>

    <t-card title="用例步骤" class="container-base-margin-top" :bordered="false">
      <t-table
        :columns="columns"
        :data="data.steps"
        :hover="hover"
        size="large"
        row-key="index"
      >
        <template #args="{ row }">
          <span>
<!--            {{ row.args }}-->
            <t-tag v-if="row.args">{{ row.args }}</t-tag>
          </span>
        </template>
        <template #op="slotProps">
          <a class="t-button-link" @click="listClick(slotProps)">管理</a>
          <a class="t-button-link" @click="deleteClickOp(slotProps)">删除</a>
        </template>
        <order-descending-icon slot="op-column"/>
      </t-table>

    </t-card>
  </div>
</template>
<script>
export default {
  name: 'DetailBase',
  data() {
    return {
      data: {},
      hover: true,
      columns: [
        {
          width: 50,
          ellipsis: true,
          colKey: 'type',
          title: '步骤类型',
        },
        {
          width: 200,
          ellipsis: true,
          colKey: 'name',
          title: '步骤名称',
        },
        {
          width: 200,
          ellipsis: true,
          colKey: 'method',
          title: '操作方法',
        },
        {
          width: 200,
          ellipsis: true,
          colKey: 'args',
          title: '方法参数',
        },
        {
          align: 'left',
          fixed: 'right',
          width: 200,
          className: 'test2',
          colKey: 'op',
          title: '操作',
        },
      ],
    };
  },
  mounted() {
    this.dataLoading = true;
    this.$request
      .get(`/api/testcases/${this.$route.params.id}`)
      .then((res) => {
        this.data = res.data;
      })
      .catch((e) => {
        console.log(e);
      })
      .finally(() => {
        // this.dataLoading = false;
      });
  },
  methods: {},
};
</script>
<style lang="less" scoped>
@import './index';
</style>
