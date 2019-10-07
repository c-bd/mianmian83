<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-row> 
        <el-col :span='6'>
          学科：
          <el-select name=""></el-select>
        </el-col>
        <el-col :span='6'>
          难度：
          <el-select></el-select>
        </el-col>
        <el-col :span='6'>
          试题类型：
          <el-select></el-select>
        </el-col>
      </el-row>
      <el-button type='success'>提交</el-button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'QuestionsList',
  data() {
    return {}
  }
}
</script>

<style scoped>
</style>
