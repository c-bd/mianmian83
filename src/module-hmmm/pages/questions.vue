<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-row type="flex">
        <el-button type="info">新增试题</el-button>
        <el-button type="info">批量导入</el-button>
      </el-row>
      <el-row>
        <el-col>
         活动区域：
            <el-select placeholder="请选择">
            </el-select>
        </el-col>
        <el-col>
         难度：
            <el-select placeholder="请选择">
            </el-select>
        </el-col>
        <el-col>
         试题类型：
            <el-select placeholder="请选择">
            </el-select>
        </el-col>
        <el-col>
         标签：
            <el-select placeholder="请选择">
            </el-select>
        </el-col>
        <el-col>
         城市：
            <el-select placeholder="请选择">
            </el-select>
        </el-col>
      </el-row>
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
