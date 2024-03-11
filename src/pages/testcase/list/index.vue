<template>
  <div>
    <t-card class="list-card-container" :bordered="false">
      <t-row justify="space-between">
        <div class="left-operation-container">
          <t-button @click="handleSetupContract"> 新建用例 </t-button>
          <t-button variant="base" theme="default" :disabled="!selectedRowKeys.length"> 导出用例 </t-button>
          <p v-if="!!selectedRowKeys.length" class="selected-count">已选{{ selectedRowKeys.length }}项</p>
        </div>
        <t-input v-model="searchValue" class="search-input" placeholder="请输入你需要搜索的内容" clearable>
          <template #suffix-icon>
            <search-icon size="20px" />
          </template>
        </t-input>
      </t-row>

      <div class="table-container">
        <t-table
          :columns="columns"
          :data="data"
          :rowKey="rowKey"
          :verticalAlign="verticalAlign"
          :hover="hover"
          :pagination="pagination"
          :selected-row-keys="selectedRowKeys"
          :loading="dataLoading"
          @page-change="rehandlePageChange"
          @change="rehandleChange"
          @select-change="rehandleSelectChange"
          :headerAffixedTop="true"
          :headerAffixProps="{ offsetTop: offsetTop, container: getContainer }"
        >
          <template #priority="{ row }">
            <t-tag v-if="row.priority === 0" theme="danger" variant="light">P0</t-tag>
            <t-tag v-if="row.priority === 1" theme="primary" variant="light">P1</t-tag>
            <t-tag v-if="row.priority === 2" theme="success" variant="light">P2</t-tag>
          </template>

          <template #op="{ row }">
            <a class="t-button-link" @click="handleClickDetail(row)">详情</a>
            <a class="t-button-link" @click="handleClickDelete(row)">删除</a>
          </template>
        </t-table>
      </div>
    </t-card>

    <t-dialog
      header="确认删除当前所选用例？"
      :body="confirmBody"
      :visible.sync="confirmVisible"
      @confirm="onConfirmDelete"
      :onCancel="onCancel"
    >
    </t-dialog>
  </div>
</template>
<script lang="ts">
import Vue from 'vue';
import { SearchIcon } from 'tdesign-icons-vue';


export default Vue.extend({
  name: 'ListBase',
  components: {
    SearchIcon,
  },
  data() {
    return {
      dataLoading: false,
      data: [],
      selectedRowKeys: [1, 2],
      value: 'first',
      columns: [
        { colKey: 'row-select', type: 'multiple', width: 64, fixed: 'left' },
        {
          title: '用例编号',
          width: 200,
          ellipsis: true,
          colKey: 'id',
        },
        {
          title: '用例名称',
          align: 'left',
          width: 250,
          ellipsis: true,
          colKey: 'name',
          fixed: 'left',
        },
        { title: '用例描述', colKey: 'description', width: 200, cell: { col: 'description' } },

        {
          title: '优先级',
          width: 200,
          ellipsis: true,
          colKey: 'priority',
        },
        {
          title: '创建人',
          width: 200,
          ellipsis: true,
          colKey: 'create_user',
        },
        {
          title: '更新人',
          width: 200,
          ellipsis: true,
          colKey: 'update_user',
        },
        {
          title: '创建时间',
          width: 200,
          ellipsis: true,
          colKey: 'create_time',
        },
        {
          title: '更新时间',
          width: 200,
          ellipsis: true,
          colKey: 'update_time',
        },
        {
          align: 'left',
          fixed: 'right',
          width: 200,
          colKey: 'op',
          title: '操作',
        },
      ],
      rowKey: 'index',
      tableLayout: 'auto',
      verticalAlign: 'top',
      hover: true,
      rowClassName: (rowKey: string) => `${rowKey}-class`,
      // 与pagination对齐
      pagination: {
        defaultPageSize: 20,
        total: 0,
        defaultCurrent: 1,
      },
      searchValue: '',
      confirmVisible: false,
      deleteIdx: -1,
      id: 0,
    };
  },
  computed: {
    confirmBody() {
      if (this.deleteIdx > -1) {
        const { name } = this.data?.[this.deleteIdx];
        return `删除后，${name}的所有用例信息将被清空，且无法恢复`;
      }
      return '';
    },
    offsetTop() {
      return this.$store.state.setting.isUseTabsRouter ? 48 : 0;
    },
  },
  mounted() {
    this.dataLoading = true;
    this.loadData();
  },

  methods: {
    loadData(){
      this.$request
        .get('/api/testcases/')
        .then((res) => {
          this.data = res.data;
          this.pagination = {
            ...this.pagination,
            total: res.data.length,
          };
        })
        .catch((e: Error) => {
          console.log(e);
        })
        .finally(() => {
          this.dataLoading = false;
        });
    },
    getContainer() {
      return document.querySelector('.tdesign-starter-layout');
    },
    rehandlePageChange(curr, pageInfo) {
      console.log('分页变化', curr, pageInfo);
    },
    rehandleSelectChange(selectedRowKeys: number[]) {
      this.selectedRowKeys = selectedRowKeys;
    },
    rehandleChange(changeParams, triggerAndData) {
      console.log('统一Change', changeParams, triggerAndData);
    },
    handleClickDetail(row) {
      this.$router.push({path: `/testcase/detail/${row.id}`});
    },
    handleSetupContract() {
      this.$router.push('/testcase/add');
    },
    handleClickDelete(row: {id: any}) {
      this.id = row.id;
      this.deleteIdx = row.rowIndex;
      this.confirmVisible = true;
    },
    onConfirmDelete() {
      // 真实业务请发起请求

      this.$request
        .delete(`/api/testcases/${this.id}/`)
        .then(() => {
          // this.data.splice(this.deleteIdx, 1);

          // this.pagination.total = this.data.length;
          // const selectedIdx = this.selectedRowKeys.indexOf(this.deleteIdx);
          // if (selectedIdx > -1) {
          //   this.selectedRowKeys.splice(selectedIdx, 1);
          // }
          this.confirmVisible = false;
          this.$message.success('删除成功');

          // this.resetIdx();
        })
        .catch((e: Error) => {
          console.log(e);
        })
        .finally(() => {
          this.loadData();
        });

    },
    onCancel() {
      this.resetIdx();
    },
    resetIdx() {
      this.deleteIdx = -1;
    },
  },
});
</script>

<style lang="less" scoped>
@import '@/style/variables';

.payment-col {
  display: flex;

  .trend-container {
    display: flex;
    align-items: center;
    margin-left: 8px;
  }
}

.left-operation-container {
  padding: 0 0 6px 0;
  margin-bottom: 16px;

  .selected-count {
    display: inline-block;
    margin-left: var(--td-comp-margin-s);
    color: var(--td-text-color-secondary);
  }
}

.search-input {
  width: 360px;
}

.t-button + .t-button {
  margin-left: var(--td-comp-margin-s);
}
</style>
