<template>
  <div class="dashboard-container">
    <!-- 两个按钮 -->
  <el-row class="dashboard-button">
    <el-button @click="open">新建标签</el-button>
    <el-button @click='skiP'>返回学科</el-button>
  </el-row>
  <!-- 模糊搜索 -->
  <el-row type="flex" align="middle" class="dashboard-input" >
    标签ID：
    <el-input style="width:240px;" v-model="DataId.id"></el-input>
    <el-button type="primary" size="small" style="margin-left:10px" @click="detailID">搜索</el-button>
  </el-row>
  <!-- 表格 -->
  <el-row type="flex" align="middle" class="dashboard-table">
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
    <el-table-column align="center" label="操作">
       <template slot-scope="scope">
        <el-button
          type="text">
          修改
        </el-button>
        <el-button
          @click ='removeState(scope.row.state)'
          type="text">
          {{scope.row.state ===1?'关闭':'开启'}}
        </el-button>
        <el-button
          type="text"
          @click='delTag(scope.row.id)'>
          删除
        </el-button>
      </template>
    </el-table-column>
  </el-table>
  </el-row>
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
import { list, add, detail, remove, removeState } from '@/api/hmmm/tags'

export default {
  name: 'TagsList',
  data() {
    return {
      DataId: {
        id: '',
        state: ''
        },
      TagsList: [],
      params: {
        total: 0, // 总条数
        page: 1, // 当前页码
        pagesize: 10 // 每页条数
      },
      data: {
        tagName: null,
        subjectID: 1
      }
    }
  },
  methods: {
      // 状态切换
     async removeState(state) {
       console.log(state)
        this.DataId.state = state === 1 ? 0 : 1
        console.log(this.DataId.state)
      },
      // 删除
     async delTag(id) {
      this.DataId.id = id
      var data = this.DataId
       await this.$confirm('此操作将永久删除该标签, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        })
      await remove(data)
          this.$message({
              message: '删除成功',
              type: 'success'
            })
          this.DataId.id = ''
          this.getDashboardTable()
      },
      // 标签ID搜索
     async detailID() {
       if (this.DataId.id === '') {
         this.$message.error('请输入标签ID')
        this.getDashboardTable()
         } else {
         var id = this.DataId.id 
       var data = this.DataId
       var str = await detail(data)
        var array = []
        if (str.data === '') {
           this.$message.error('您输入的ID不存在') 
           this.getDashboardTable()
        } else {
          array.push(str.data)
           this.TagsList = array
           this.params.total = 1
        } 
} 
      },
      // 新建标签
    async open() {
       var value = await this.$prompt('请输入标签名', '新建标签', {
          confirmButtonText: '确定',
          cancelButtonText: '取消'
        })
            this.data.tagName = value.value
            var data = this.data
            await add(data)
            this.$message({
              message: '新建成功',
              type: 'success'
            })
           this.getDashboardTable()
      },
      // 返回学科
    skiP() {
      this.$router.push('/subjects/list')
    },
      // 获取标签列表
   async getDashboardTable(data) {
      var str = await list(data)
      this.TagsList = str.data.items
      this.params.total = str.data.counts
    },
    // 分页
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
