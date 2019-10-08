<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-table
      :data="randomslist"
      border
      style="width: 100%">
    <el-table-column
      prop="id"
      label="序号"
      width="80">
    </el-table-column>
    <el-table-column
      prop="addTime"
      label="组题时间"
      width="160">
    </el-table-column>
    <el-table-column
      prop="userName"
      label="用户名"
      width="120">
    </el-table-column>
    <el-table-column
      prop="questionIDs"
      label="试题ID">
    </el-table-column>
    <el-table-column
      prop="progressOfAnswer"
      label="答题进度"
      width="100">
    </el-table-column>
    <el-table-column
      prop="accuracyRate"
      label="正确率">
    </el-table-column>
    <el-table-column
      prop="totalSeconds"
      label="答题总耗时(秒)">
    </el-table-column>
    <el-table-column
      prop="questionType"
      label="组题类型/详情">
    </el-table-column>
    <el-table-column
      label="操作">
      <template>
        <el-button type="text" @click="deletelist(id)">删除</el-button>
      </template>
    </el-table-column>
  </el-table>
    </div>
  </div>
</template>

<script>
// 把api数据相关接口方法导入
import { randoms, removeRandoms } from '@/api/hmmm/questions'
export default {
  name: 'QuestionsRandoms',
  data() {
    return {
      randomslist: []
    }
  },
  methods: {
    // 获取组题列表数据
    async getrandomslist() {
      var rst = await randoms()
      console.log(rst) 
      this.randomslist = rst.data.items
      console.log(rst.data.items)
    },
    deletelist(id) {
      this.$confirm('此操作将永久删除该条数据, 是否继续?', '提示').then(async () => {
        await deletelist({id: id}).then(response => {
          // 响应回来的数据
          this.$message.success('成功删除该条数据')
          // 后台数据删除后，将页面上的数据也删除
          this.randomslist.splice(id, 1)
        }).catch(response => {
          this.$message.error('删除失败')
        }).catch(() => {
          this.$message.info('已取消删除')
        })
      })
    }
  },
  created() {
    this.getrandomslist()
  }
}
</script>

<style scoped>
  
</style>
