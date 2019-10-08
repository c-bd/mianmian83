<template>
  <div class="dashboard-container">
     <el-table
    :data="tableData"
    border
    style="width: 100%">
    <el-table-column
      prop="date"
      label="序号"
      width="50">
    </el-table-column>
    <el-table-column
      prop="name"
      label="组题时间"
      width="100">
    </el-table-column>
    <el-table-column
      prop="address"
      label="用户名">
    </el-table-column>
    <el-table-column
      prop="address"
      label="试题ID">
    </el-table-column>
    <el-table-column
      prop="address"
      label="答案进度">
    </el-table-column>
    <el-table-column
      prop="address"
      label="正确率">
    </el-table-column>
    <el-table-column
      prop="address"
      label="答题总耗时（秒）">
    </el-table-column>
    <el-table-column
      prop="address"
      label="组题类型操作/详情">
    </el-table-column>
    <el-table-column
      prop="address"
      label="操作">
    </el-table-column>
  </el-table>
</div>
</template>

<script>
export default {
  name: 'QuestionsRandoms',
  data() {
    return {
       tableData: [],
       
    }
  }
}
</script>

<style scoped>
</style>
