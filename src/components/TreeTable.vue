<template>
  <div ref="container" class="mt10">
    <el-row class="mb10">
      <el-col :span="16">
        <slot name="header"></slot>
      </el-col>
      <el-col :span="8" class="tr">
        <el-button @click="toggleRowExpansion(true)" plain>展开</el-button>
        <el-button @click="toggleRowExpansion(false)" plain>收起</el-button>
      </el-col>
    </el-row>
    <slot name="extra"></slot>
    <el-table
      :data="data.list"
      row-key="code"
      default-expand-all
      :tree-props="{ children: 'children', hasChildren: 'hasChildren' }"
      ref="table"
      @row-click="handleRowClick"
    >
      <el-table-column prop="code" label="科目编码" width="200"></el-table-column>
      <el-table-column prop="name" label="名称" width="160"></el-table-column>
      <el-table-column :prop="item.key" :label="item.name" v-for="(item, index) in data.columns" :key="item.name">
        <template slot-scope="{ row }">
          {{ row.extends[index].value }}
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>
<script>
export default {
  name: 'TreeTable',
  props: {
    data: {
      type: Object,
      required: true,
    },
  },
  mounted() {
    console.log(this.data);
  },
  methods: {
    handleRowClick(row) {
      if (row.children) {
        this.$refs.table.toggleRowExpansion(row);
      }
    },
    toggleRowExpansion(status) {
      // element-ui 的 expend-row-keys 只支持 expand columns，不支持 tree table
      // 手动获取节点点击
      let nodeList = [];
      if (status) {
        // 全部展开
        nodeList = this.$refs.container.querySelectorAll(
          '.el-table__body-wrapper .el-table__expand-icon:not(.el-table__expand-icon--expanded)',
        );
      } else {
        let qs = '';
        if (this.collapseToTopLevel) {
          qs = '.el-table__body-wrapper .el-table__row .el-table__expand-icon--expanded';
        } else {
          qs = '.el-table__body-wrapper .el-table__row:not(.el-table__row--level-0) .el-table__expand-icon--expanded';
        }
        // 全部收起不包括顶级科目
        nodeList = this.$refs.container.querySelectorAll(qs);
      }

      for (let i = 0, len = nodeList.length; i < len; i += 1) {
        nodeList[i].click();
      }
    },
  },
};
</script>
<style scoped>
.el-table--border th,
.el-table--border td {
  border: 0;
}
</style>
