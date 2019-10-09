<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-button @click="addsubject">新增试题</el-button>
      <el-button>批量导入</el-button>
      <el-form label-width="80px" style='text-align:center' :model="loginForm" ref="ruleForm">
        <el-row type="flex" style="margin:10px 0px 0px 0px">
          <el-form-item label="学科" style="background-color:#ddd;margin-right:8px" prop="subjectID">
            <el-select v-model="loginForm.subjectID">
              <el-option v-for="item in subjectIDList" :key="item.value" :label="item.label" :value="item.value"></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="难度" style="background-color:#ddd;margin-right:8px" prop="difficulty">
            <el-select v-model="loginForm.difficulty">
               <el-option v-for="item in difficultyList" :key="item.value" :label="item.label" :value="item.value"></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="试题类型" style="background-color:#ddd;margin-right:8px" prop="questionType">
            <el-select v-model="loginForm.questionType">
              <el-option v-for="item in questionTypeList" :key="item.value" :label="item.label" :value="item.value"></el-option>
            </el-select>
          </el-form-item>
           <el-form-item label="标签" style="background-color:#ddd;margin-right:8px" prop="tags">
            <el-select v-model="loginForm.tags">
              <el-option v-for="item in tagsList" :key="item.value" :label="item.label" :value="item.value"></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="城市" style="background-color:#ddd;margin-right:8px" prop="province">
            <el-select v-model="loginForm.province" @change='getcity'>
                                  <!-- 这里可以直接调用methos方法 不是仅仅可以调用data数据 -->
              <el-option v-for="(item,index) in provinces()" :key="index" :label="item" :value="item"></el-option>
            </el-select>
          </el-form-item >
           <el-form-item prop="city">
            <el-select v-model="loginForm.city">
              <el-option v-for="(item,index) in cityList" :key="index" :label="item" :value="item"></el-option>
            </el-select>
          </el-form-item>
        </el-row>
        <el-row type='flex'>
           <el-form-item label="关键字" style="background-color:#ddd;margin-right:8px" prop="keyword">
              <el-input></el-input>
          </el-form-item>
          <el-form-item label="题目备注" style="background-color:#ddd;margin-right:8px" prop="remarks">
            <el-input></el-input>
          </el-form-item>
          <el-form-item label="企业简称" style="background-color:#ddd;margin-right:8px" prop="shortName">
            <el-select v-model="loginForm.shortName">
              <el-option v-for="item in shortNameList" :key="item.value" :label="item.label" :value="item.value"></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="方向" style="background-color:#ddd;margin-right:8px" prop="direction">
            <el-select v-model="loginForm.direction">
              <el-option v-for="(item,index) in directionList" :key="index" :value="item" :label="item"></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="录入人" style="background-color:#ddd;margin-right:8px" prop="creatorID">
            <el-select v-model="loginForm.creatorID">
              <el-option v-for="(item,index) in creatorIDList" :key="index" :label="item" :value="item"></el-option>
            </el-select>
          </el-form-item>
        </el-row>
        <el-row type="flex">
           <el-form-item label="二级目录" style="background-color:#ddd;margin-right:8px;float:left" prop="catalogID">
            <el-select v-model="loginForm.catalogID">
              <el-option v-for="item in catalogIDList" :key="item.value" :label="item.label" :value="item.value"></el-option>
            </el-select>
          </el-form-item>
          <div style="margin-left:700px">
            <el-button type="danger" @click="cleareSeek">清除</el-button>
            <el-button type="primary" @click="getlist">搜索</el-button>
          </div>        
        </el-row>
      </el-form>  
           <el-table
      :data="tableData"
      stripe
      border
      style="width: 100%">
      <el-table-column
        prop="id"
        label="序号"
        width="60">
      </el-table-column>
      <el-table-column
        prop="number"
        label="试题编号"
        width="120">
      </el-table-column>
      <el-table-column
        prop="subject"
        label="学科"
        width='80'>
      </el-table-column>
      <el-table-column
        prop="questionType"
        label="题型"
        width='80' :formatter="questionTypeFMT">
      </el-table-column>
      <el-table-column
        prop="question"
        label="题干"
        width='150'>
      </el-table-column>
      <el-table-column
        label="录入时间"
        width='160' >
      <template slot-scope="scope">
        <span>{{scope.row.addDate | parseTimeByString("{y}-{m}-{d} {h}:{m}:{s}")}}</span>
      </template>
      </el-table-column>
      <el-table-column
        prop="difficulty"
        label="难度"
        width='50' :formatter="difficultyFMT">
      </el-table-column>
      <el-table-column
        prop="address"
        label="使用次数"
        width='80'>
      </el-table-column>
      <el-table-column
        prop="creator"
        label="录入人"
        width='130'>
      </el-table-column>
      <el-table-column label="操作">
          <template slot-scope="obj">
             <el-button type="text" size="small">预览</el-button> 
             <el-button type="text" size="small" @click="altertsxt(obj.row)">修改</el-button>
             <el-button type="text" size="small"             
             @click="deletelist(obj.row)">删除</el-button>
             <el-button type="text" size="small" @click='addselection(obj.row,publish)'>{{pubulish==='1'?'已添加':'加入精选'}}</el-button>
          </template>
      </el-table-column>
    </el-table> 
    </div>
     <div class="block">

    <el-row type="flex" justify="center">
    <el-pagination
      background
      layout="total, sizes, prev, pager, next, jumper"
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="loginForm.page"
      :page-size="loginForm.pageSize"
      :total="loginForm.total">
    </el-pagination>
    </el-row>

  </div>
  </div>
  
</template>
<script>
import {simple as simpled} from '@/api/base/users'// 用户列表
import {simple as simplesd} from '@/api/hmmm/directorys'// 目录列表
import {simple} from '@/api/hmmm/subjects'// 学科列表
import {simple as simples} from '@/api/hmmm/tags'// 标签列表
import {list, remove, choicePublish} from '@/api/hmmm/questions'// 基础题库列表
// 起个别名给这个数组 直接当作值传入就可以了
import {provinces, citys} from '@/api/hmmm/citys'// 城市列表
import {difficulty as difficultyList, questionType as questionTypeList, direction as directionList} from '@/api/hmmm/constants'// 常量列表
export default {
  name: 'QuestionsList',
  data() {
    return {
      publish: '1',    
      tableData: [],
      subjectIDList: [],
      difficultyList,
      questionTypeList,
      tagsList: [],
      cityList: [],
      keywordList: [],
      remarksList: [],
      shortNameList: [],
      directionList,
      creatorIDList: [],
      catalogIDList: [],
      loginForm: {
        total: 0,
        page: 1,
        pagesize: 10,
        subjectID: '',
        difficulty: '',
        questionType: '',
        tags: '',
        province: '',
        city: '',
        keyword: '',
        remarks: '',
        shortName: '',
        direction: '',
        creatorID: '',
        catalogID: ''
      }
    }
  },
  created() {
    this.getsubjectID()// 学科获取
    this.getdifficulty()// 难度获取
    this.getlist()// 题库列表
    this.getcatalogID()// 二级目录
    this.getuser()// 用户列表
  },
  methods: {
    addsubject() {
      this.$router.push('/questions/new')
    },
    // 清除搜索列表全部
    cleareSeek() {
      this.loginForm = ''
    },
    // 用户列表
    async getuser() {
        let ref = await simpled()         
        this.creatorIDList = ref.data 
    },
    // ------------------------------阻止默认跳转@click.prevent
    // 更改题型数字为文字
    questionTypeFMT(row, colum, cellValue) {
        return questionTypeList[cellValue - 1]['label']       
    },
    // 更改难度数字类型为文字
    difficultyFMT(row, colum, cellValue) {
        return difficultyList[cellValue - 1]['label']
    },
    // 分页
     handleCurrentChange (val) {
      this.loginForm.page = val
      this.getlist()
      //  这时候当我们改变页码时 会传入一个参数这个参数就是当前页码的数  我们把这个数重新传入  然后发送请求  就会把这一夜的数据给你返回来
    },
    // 分页
     handleSizeChange (val) {
      this.loginForm.pageSize = val
      this.getlist()
    },
    // 获取学科列表
    async getsubjectID() {
      let ref = await simple()
      this.subjectIDList = ref.data
      
    },
    // 难度列表获取
    async getdifficulty() {
      let ref = await simples()
      this.tagsList = ref.data
    },
    // 拉取列表
    async getlist() {  
      let ref = await list(this.loginForm)
      this.tableData = ref.data.items         
      this.loginForm.total = ref.data.pages
      
    },
    // 获取省份
    provinces, // 名字和名字相同 
  //  获取城市
   getcity() {
     this.loginForm.city = ''
     let ref = citys(this.loginForm.province)
     this.cityList = ref     
   },
  //  删除文章列表
   async deletelist(id) {
     this.$confirm('你确定要删除吗？')
     await remove(id)    
     this.$message('删除成功')
     this.getlist()
    },
    // 加入精选
    async addselection(data, publish) {
    let ref = await choicePublish(data, publish)
    if (ref.data.success) {
    this.$message('加入精选完成')
    this.$router.push('/questions/choice')
    } else {
      this.$message('添加失败')
    }   
    },
    // 文章修改
    async altertsxt(data) {
      this.$router.push(`/questions/new/${data.id}`)     
    },
    async getcatalogID() {
      let ref = await simplesd()
      this.catalogIDList = ref.data
    }
  }
}
</script>

<style scoped>
</style>
