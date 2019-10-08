<template>
  <div class="dashboard-container">
    <el-form style="margin-left:30px;margin-top:30px" :model="fromData" status-icon :rules="loginRules" ref="ruleForm"  >
      <!-- 学科 -->
      <el-form-item label="学科：" prop="subjectID">
        <el-select placeholder="请选择" style="width:440px" v-model="fromData.subjectID">
          <el-option
            v-for="item in subjectIDList"
            :key="item.value"
            :value="item.value"
            :label="item.label"
          ></el-option>
        </el-select>
      </el-form-item>
      <!-- 目录 -->
      <el-form-item label="目录：" prop="catalogID">
        <el-select placeholder="请选择" style="width:440px" v-model="fromData.catalogID">
          <el-option
            v-for="item in catalogIDList"
            :key="item.value"
            :value="item.value"
            :label="item.label"
          ></el-option>
        </el-select>
      </el-form-item>
      <!-- 企业 -->
      <el-form-item label="企业：" prop="enterpriseID"> 
        <el-select placeholder="请选择" style="width:440px" v-model="fromData.enterpriseID">
          <el-option v-for="item in shortNameList " :key="item.value" :value="item"></el-option>
        </el-select>
      </el-form-item>
      <!-- 城市 -->
       <el-form-item label="城市: " prop="province">
            <el-select v-model="fromData.province" @change='getcity' style="width:213px;margin-left:10px">
                                  <!-- 这里可以直接调用methos方法 不是仅仅可以调用data数据 -->
              <el-option v-for="(item,index) in provinces()" :key="index" :label="item" :value="item"></el-option>
            </el-select>
             <el-select v-model="fromData.city">
              <el-option v-for="(item,index) in cityList" :key="index" :label="item" :value="item"></el-option>
            </el-select>
          </el-form-item >
           
      <!-- 方向 -->
      <el-form-item label="方向：" prop="direction">
        <el-select placeholder="请选择" style="width:440px" v-model="fromData.direction">
          <el-option v-for="(item,index) in  directionList" :key="index" :value="item"></el-option>
        </el-select>
      </el-form-item>
      <!-- 题型 -->
      <el-form-item label="题型：" prop="questionType">
      <el-radio-group v-model="fromData.questionType">
        <el-radio :label="1">单选</el-radio>
        <el-radio :label="2">多选</el-radio>
        <el-radio :label="3">简答</el-radio>
      </el-radio-group>
      </el-form-item>
      <!-- 难度 -->
         <el-form-item label="难度：" prop="difficulty">
        <el-radio-group v-model="fromData.difficulty">
          <el-radio :label="1">简单</el-radio>
          <el-radio :label="2">一般</el-radio>
          <el-radio :label="3">困难</el-radio>
        </el-radio-group>
         </el-form-item>
        
      <!-- 文本域 -->
      题干：
      <el-input
    style="margin-top:30px"
  type="textarea"
  :rows="10"
  placeholder="请输入内容"
  v-model="textarea">
</el-input>
<!-- 选项 -->
      <el-form-item label="选项：" prop="option">
          <el-radio-group style="margin-top:50px;margin-left:-53px" v-model="fromData.option">
            <div style="margin-top:10px">
              <el-radio :label="1">
            A:    <el-input></el-input>
              </el-radio>      
            </div>
            <div style="margin-top:10px">
              <el-radio :label="2">
            B:    <el-input></el-input>
               </el-radio>       
           </div>
           <div style="margin-top:10px">
              <el-radio :label="3">          
            C:    <el-input></el-input>
              </el-radio>
           </div>
           <div style="margin-top:10px">
              <el-radio :label="4">
            D:    <el-input></el-input>
              </el-radio>
            </div>
            <el-button type="info" plain style="margin-top:30px">+增加选项与答案</el-button>
            </el-radio-group>     
      </el-form-item>      
      <!-- 答案解析文本域 -->
      答案解析：
        <el-input
    style="margin-top:30px"
  type="textarea"
  :rows="10"
  placeholder="请输入内容"
  v-model="textarea">
</el-input>
<!-- 题目备注文本域 -->
题目备注：
  <el-input
    style="margin-top:30px"
  type="textarea"
  :rows="10"
  placeholder="请输备注"
  v-model="textarea">
</el-input>
  <el-form-item label="试题标签：" style='margin-top:20px' prop="tags">
    <el-input style="width:30%"  placeholder="请输入" v-model="fromData.tags"></el-input>
  </el-form-item>
    </el-form>
    <div style="margin-left:500px">
    <el-button type="primary" @click="publish">提交</el-button>
    <el-button>取消</el-button>
    </div>
  </div>
</template>

<script>
// 基础题库详情
import {detail, update} from '@/api/hmmm/questions'
// 学科
import { simple } from '@/api/hmmm/subjects'
// 目录
import { simple as simples } from '@/api/hmmm/directorys'
// 城市
import { provinces, citys } from '@/api/hmmm/citys'
// 题型  // 难度  //方向
import {
  questionType as questionTypeList,
  difficulty as difficultyList,
  direction as directionList
} from '@/api/hmmm/constants'
// 企业
import { list } from '@/api/hmmm/companys'

export default {
  name: 'QuestionsNew',
  data() {
    return {
      subjectIDList: [],
      catalogIDList: [],
      directionList,    
      shortNameList: [],
      cityList: [],
      fromData: {
        subjectID: '',
        catalogID: '',
        shortName: '',
        direction: 1,
        province: '',
        city: '',
        questionType: 1,
        difficulty: 2,
        options: 2,
        tags: ''
      },
      loginRules: {
        subjectID: [
          { required: true, message: '请选择学科' }
        ],
        catalogID: [
          { required: true, message: '请选择目录' }
        ],
        enterpriseID: [
          { required: true, message: '请选择企业' }
        ],
        province: [
          { required: true, message: '请选择省' }
        ],
        city: [
           { required: true, message: '请选择市' }
        ],
        direction: [
          { required: true, message: '请选择方向' }
        ],
        questionTypeList: [
          { required: true, message: '题型不能为空' }
        ],
        difficultyList: [
          { required: true, message: '请选择难度' }
        ],
        tags: [
          { required: true, message: '请输入试题标签' }
        ]

      }
    }
  },
  methods: {
    async publish() {
      let { articleId } = this.$route.params      
      let ref = await update(articleId)
      debugger
      
    },
        // 根据id获取数据详情
      async getQuestion(articleId) {
       let ref = await detail(articleId)
       this.fromData = ref.data
      },
    // 获取县区的值
     getcity() {
     this.fromData.city = ''
     let ref = citys(this.fromData.province)
     this.cityList = ref     
   },
    // 获取学科
    async getSubjectIDList() {
      var rst = await simple()
      this.subjectIDList = rst.data
    },
    // 获取目录
    async getcatalogIDList() {
      var rts = await simples()
      this.catalogIDList = rts.data
    },
    // 获取企业
    async getshortNameList() {
      var rss = await list()   
      this.shortNameList = rss.data.items[0].shortName     
    },
    // 获取城市
      provinces
  },
  created() {
    // 获取动态路由参数
    let { articleId } = this.$route.params
    this.getQuestion(articleId)// 获取基础题库详情
    this.getSubjectIDList()
    this.getcatalogIDList()
    this.getshortNameList()
  }
}
</script>

<style scoped>

</style>
