<template>
  <div class="dashboard-container">
    <!-- 两个按钮 -->
  <el-row class="dashboard-button">
    <el-button >新建标签</el-button>
    <el-button @click='skiP'>返回学科</el-button>
  </el-row>
  <!-- 模糊搜索 -->
  <el-row type="flex" align="middle" class="dashboard-input" >
    标签名称：
    <el-input style="width:240px;"></el-input>
  </el-row>
  <!-- 表格 -->
  <el-table class="dashboard-table" :data="TagsList">
    <el-table-column align="center" label="序号"  prop="id"></el-table-column>
    <el-table-column align="center" label="标签名称"  prop="tagName"></el-table-column>
    <el-table-column align="center" label="创建者" prop="username"></el-table-column>
    <el-table-column align="center" label="创建日期">
      <template slot-scope="scope">
        <span>{{scope.row.addDate | parseTimeByString("{y}-{m}-{d} {h}:{m}:{s}")}}</span>
      </template>
    </el-table-column>
    <el-table-column align="center" label="面试题数量"  prop="totals"></el-table-column>
    <el-table-column align="center" label="状态">
       <template slot-scope="scope">
        <span>{{scope.row.state===1?'开启':'屏蔽'}}</span>
      </template>
    </el-table-column>
    <el-table-column align="center" fixed='right' label="操作">
       <template>
        <el-button
          type="text"
          size="small">
          移除
        </el-button>
        <el-button
          type="text"
          size="small">
          禁用
        </el-button>
        <el-button
          type="text"
          size="small">
          删除
        </el-button>
      </template>
    </el-table-column>
  </el-table>
  <!-- 分页 -->
  <el-row type="flex" justify="center" >
    <el-pagination
  @current-change="pageChange"
  :page-size="params.pagesize"
  background
  layout="prev, pager, next"
  :current-page="params.page"
  :total="params.total">
</el-pagination>
  </el-row>
  </div>
</template>

<script>
import { list } from '@/api/hmmm/tags'
export default {
  name: 'TagsList',
  data() {
    return {
      TagsList: [],
      params: {
        total: 0, // 总条数
        page: 1, // 当前页码
        pagesize: 10 // 每页条数
      }
    }
  },
  methods: {
    skiP() {
      this.$router.push('/subjects/list')
    },
   async getDashboardTable(data) {
      var str = await list(data)
      console.log(str)
      this.TagsList = str.data.items
      this.params.total = str.data.counts
    },
    pageChange(newPage) {
      this.params.page = newPage
      var params = this.params
      this.getDashboardTable(params)
    }
  },

  created() {
    this.getDashboardTable()
  }
}
</script>

<style scoped>
.dashboard-container{
  margin-left:20px;
  margin-top:20px;
  
}
.dashboard-button,.dashboard-input,.dashboard-table{
    margin:20px 20px
  }
.dashboard-table{
 width:100%px;
background:rgba(255,255,255,1);
border-radius:5px;
}
.dashboard-input{
  padding-left: 20px;
  width:100%px;
height:75px;
background:rgba(255,255,255,1);
border-radius:5px;
}

</style>
