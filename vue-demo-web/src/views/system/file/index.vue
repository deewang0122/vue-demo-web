<template>
  <base-wrapper>
    <base-header>
      <el-input v-model="listQuery.name" placeholder="请输入文件名" style="width: 200px" clearable @clear="refreshTableData" />
      <el-button type="primary" @click="refreshTableData">查询</el-button>
    </base-header>

    <base-table-p ref="baseTableRef" api="sys_file.page" :params="listQuery">
      <el-table-column label="ID" prop="id" align="center" />
      <el-table-column label="文件名" prop="name" align="center" />
      <el-table-column label="文件地址" prop="url" align="center">
        <template #default="scope"> <el-image :src="scope.row.url" class="img-sm" /></template>
      </el-table-column>
      <!-- <el-table-column label="文件唯一标识（md5）" prop="md5" align="center" /> -->
      <el-table-column label="文件大小" prop="size" align="center">
        <template #default="scope">
          <span>{{ (scope.row.size / 1024 / 1024).toFixed(2) }}M</span>
        </template>
      </el-table-column>
      <el-table-column label="创建时间" prop="createTime" align="center" />
    </base-table-p>
  </base-wrapper>
</template>

<script setup>
const { proxy } = getCurrentInstance();
let listQuery = $ref({});

function refreshTableData() {
  proxy.$refs.baseTableRef.refresh();
}
</script>

<style lang="scss" scoped></style>
