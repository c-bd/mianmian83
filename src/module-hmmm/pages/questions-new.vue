<template>
  <div class="dashboard-container">
    <el-form style="margin-left:30px;margin-top:30px" :model="fromData">
      <!-- 学科 -->
      <el-form-item>
        学科：
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
      <el-form-item>
        目录：
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
      <el-form-item>
        企业：
        <el-select placeholder="请选择" style="width:440px">
          <el-option v-for="item in shortNameList " :key="item.value" :value="item"></el-option>
        </el-select>
      </el-form-item>
      <!-- 城市 -->
      <el-form-item>
        城市：
        <el-select placeholder="城市" @change="getCitys">
          <el-option v-for="item in provinces() " :key="item" :label="item" :value="item"></el-option>
        </el-select>
        <el-select placeholder="区县">
          <el-option></el-option>
        </el-select>
      </el-form-item>
      <!-- 方向 -->
      <el-form-item>
        方向：
        <el-select placeholder="请选择" style="width:440px" v-model="fromData.direction">
          <el-option v-for="item in  directionList" :key="item.value" :value="item"></el-option>
        </el-select>
      </el-form-item>
      <!-- 题型 -->
      题型：
      <el-radio-group v-model="questionTypeList">
        <el-radio :label="1">单选</el-radio>
        <el-radio :label="2">多选</el-radio>
        <el-radio :label="3">简答</el-radio>
      </el-radio-group>
      <!-- 难度 -->
      <div style="margin-top:30px">
        难度：
        <el-radio-group v-model="difficultyList">
          <el-radio :label="1">简单</el-radio>
          <el-radio :label="2">一般</el-radio>
          <el-radio :label="3">困难</el-radio>
        </el-radio-group>
      </div>
      <!-- 文本域 -->
      <el-input
    style="margin-top:30px"
  type="textarea"
  :rows="10"
  placeholder="请输入内容"
  v-model="textarea">
</el-input>
<!-- 选项 -->
      <el-radio-group style="margin-top:30px">
        <el-radio>
           A: <el-input></el-input>
        </el-radio>
      </el-radio-group>
      <div>
         <el-radio-group style="margin-top:30px">
        <el-radio>
          
           B: <el-input></el-input>
        </el-radio>
      </el-radio-group>
      </div>
      <div>
         <el-radio-group style="margin-top:30px">
        <el-radio>          
           C: <el-input></el-input>
        </el-radio>
      </el-radio-group>
      </div>
      <div>
         <el-radio-group style="margin-top:30px">
        <el-radio>
           D: <el-input></el-input>
        </el-radio>
      </el-radio-group>
      </div>
      <el-button type="info" plain style="margin-top:30px">+增加选项与答案</el-button>
      <!-- 答案解析文本域 -->
        <el-input
    style="margin-top:30px"
  type="textarea"
  :rows="10"
  placeholder="请输入内容"
  v-model="textarea">
</el-input>
<!-- 题目备注文本域 -->
  <el-input
    style="margin-top:30px"
  type="textarea"
  :rows="10"
  placeholder="请输备注"
  v-model="textarea">
</el-input>
  <el-form-item>
    试题标签：<el-input style="width:30%; margin-top:20px"  placeholder="请输入"></el-input>
  </el-form-item>
    </el-form>
    <div style="margin-left:500px">
    <el-button type="primary">提交</el-button>
    <el-button>取消</el-button>
    </div>
  </div>
</template>

<script>
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
      questionTypeList: 1,
      difficultyList: 2,
      shortNameList: [],

      fromData: {
        subjectID: '',
        catalogID: '',
        questionType: '',
        shortName: '',
        direction: ''

      }
    }
  },
  methods: {
    // 获取学科
    async getSubjectIDList() {
      var rst = await simple()
      this.subjectIDList = rst.data
    },
    // 获取目录
    async getcatalogIDList() {
      var rts = await simples()
      console.log(rts)
      this.catalogIDList = rts.data
    },
    // 获取企业
    async getshortNameList() {
      var rss = await list()
        console.log(rss)
        
         this.shortNameList = rss.data.items[0]
         console.log(this.shortNameList)       
    },
    // 获取城市
      provinces
  },
  created() {
    this.getSubjectIDList()
    this.getcatalogIDList()
    this.getshortNameList()
  }
}
</script>

<style scoped>
</style>
