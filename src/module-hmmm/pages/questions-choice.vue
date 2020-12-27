<template>
  <div class="container">
    <el-card class="box-card">
      <div class="right-shiti">
        <span>说明</span>
        <el-button
          type="button"
          class="el-button el-button--success el-button--small"
          size="small"
          @click="$router.push('/questions/new')"
        >
          <i class="el-icon-edit"></i>
          <span>新增题库</span>
        </el-button>
      </div>

      <!-- 第一行区域 -->
      <el-form ref="FormRefs" label-width="80px" :model="searchForm">
        <el-row>
          <!-- 学科 -->
          <el-col :span="6">
            <el-form-item prop="subjectID" label="学科" size="small">
              <el-select
                v-model="searchForm.subjectID"
                placeholder="请选择"
                @change="subChange"
                style="width:100%"
              >
                <el-option
                  v-for="(item, index) in subList"
                  :key="index"
                  :label="item.label"
                  :value="item.value"
                ></el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <!-- 二级目录 -->
          <el-col :span="6">
            <el-form-item
              prop="catalogID"
              label="二级目录"
              label-width="80px"
              size="small"
            >
              <el-select v-model="searchForm.catalogID" placeholder="请选择" style="width:100%">
                <el-option
                  v-for="(item,index) in twoSubList"
                  :key="index"
                  :label="item.directoryName"
                  :value="item"
                >
                </el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <!-- 标签 -->
          <el-col :span="6">
            <el-form-item label="标签" label-width="80px" size="small">
              <el-select v-model="searchForm.tagsValue" placeholder="请选择" style="width:100%">
                <el-option
                  v-for="(item, index) in tagLists"
                  :key="index"
                  :label="item.tagName"
                  :value="item.id"
                >
                </el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <!-- 关键词 -->
          <el-col :span="6">
            <el-form-item label="关键词" label-width="80px">
              <el-input size="small"></el-input>
            </el-form-item>
          </el-col>
        </el-row>

        <!-- 第二行 -->
        <el-row>
          <!-- 试题类型 -->
          <el-col :span="6">
            <el-form-item
              prop="questionType"
              label="试题类型"
              size="small"
              label-width="80px"

            >
              <el-select v-model="searchForm.quest" placeholder="请选择"  style="width:100%">
                <el-option
                  v-for="(item, index) in questionType"
                  :key="index"
                  :label="item.label"
                  :value="item.value"
                ></el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <!-- 难度 -->
          <el-col :span="6">
            <el-form-item label="难度" size="small">
              <el-select v-model="searchForm.diff" placeholder="请选择" style="width:100%">
                <el-option
                  v-for="(item, index) in difficulty"
                  :key="index"
                  :label="item.label"
                  :value="item.value"
                ></el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <!-- 方向 -->
          <el-col :span="6">
            <el-form-item
              prop="direction"
              label="方向"
              size="small"
              label-width="80px"
            >
              <el-select v-model="searchForm.dir" placeholder="请选择" style="width:100%">
                <el-option
                  v-for="(item, index) in direction"
                  :key="index"
                  :label="item"
                  :value="index"
                ></el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <!-- 录入人 -->
          <el-col :span="6">
            <el-form-item
              prop="creatorID"
              label="录入人"
              size="small"
              label-width="80px"
            >
              <el-select v-model="searchForm.creators" style="width:100%">
                <el-option
                  v-for="creatorItem in querestlist"
                  :key="creatorItem.id"
                  :label="creatorItem.username"
                  :value="creatorItem.id"
                ></el-option>
              </el-select>
            </el-form-item>
          </el-col>
        </el-row>

        <!-- 第三行 -->
        <el-row>
          <!--  题目备注 -->
          <el-col :span="6">
            <el-form-item
              prop="remarks"
              label="题目备注"
              label-width="80px"
              size="small"
            >
              <el-input v-model="searchForm.remarks"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <!-- 企业简称 -->
            <el-form-item
              prop="shortName"
              label="企业简称"
              label-width="80px"
              size="small"
            >
              <el-input v-model="searchForm.shortName"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <!-- 城市 -->
            <el-row>
              <el-col>
                <el-form-item prop="province" label="城市" label-width="80px">
                  <el-select
                    v-model="searchForm.province"
                    placeholder="请选择"
                    size="small"
                    style="width: 45%; margin-right: 2%"
                    @change="cityChange"
                  >
                    <el-option
                      v-for="(item, index) in provinces"
                      :key="index"
                      :label="item"
                      :value="item"
                    >
                    </el-option>
                  </el-select>
                  <el-select
                    v-model="searchForm.city"
                    placeholder="请选择"
                    size="small"
                    style="width:53%"
                    @change="cityChange"
                  >
                    <el-option
                      v-for="(item, index) in citys"
                      :key="index"
                      :label="item"
                      :value="item"
                    >
                    </el-option>
                  </el-select>
                </el-form-item>
                <el-form-item prop="city"></el-form-item>
              </el-col>
            </el-row>
          </el-col>
          <el-col :span="6">
            <el-form-item style="text-align: right">
              <el-button size="small" @click="clearSearchForm">清除</el-button>
              <el-button type="primary" size="small">搜索</el-button>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>

      <!-- 选项卡区域 -->
      <el-tabs v-model="activeName" type="card" @tab-click="handleClick">
        <el-tab-pane label="全部" name="0">
          <q-table
            @question-preview="previewQ"
            @edit-question="$router.push('/questions/new')"
            :questionBank="allQuestion"
            :conuts="allState"
            :query="query"
            @update-size="allQuestions()"
            @update-page="allQuestions()"
          ></q-table>
        </el-tab-pane>
        <el-tab-pane label="待审核" name="1">
          <q-table
            @question-preview="previewQ"
            @edit-question="$router.push('/questions/new')"
            :questionBank="pendingQuestion"
            :conuts="pendingState"
            :query="query"
            @update-size="pendingQuestions()"
            @update-page="pendingQuestions()"
          ></q-table>
        </el-tab-pane>
        <el-tab-pane label="已审核" name="2">
          <q-table
            @question-preview="previewQ"
            @edit-question="$router.push('/questions/new')"
            :questionBank="checkedQuestion"
            :conuts="checkedState"
            :query="query"
            @update-size="checkedQuestions()"
            @update-page="checkedQuestions()"
          ></q-table>
        </el-tab-pane>
        <el-tab-pane label="已拒绝" name="3">
          <q-table
            @question-preview="previewQ"
            @edit-question="$router.push('/questions/new')"
            :questionBank="rejectQuestion"
            :conuts="rejectState"
            :query="query"
            @update-size="rejectQuestions()"
            @update-page="rejectQuestions()"
          ></q-table>
        </el-tab-pane>
      </el-tabs>
      <!-- 预览试题对话框-->
      <el-dialog title="提示" width="50%" :visible.sync="previewDialog">
        <!-- 预览试题内容组件 -->
            <q-preview :checkedId="choiceId"></q-preview>
        <span slot="footer" class="dialog-footer">
          <el-button type="primary" @click="previewDialog = false">
            关闭
          </el-button>
        </span>
      </el-dialog>
    </el-card>
  </div>
</template>

<script>
import QTags from '../components/questions-tags'
import QTable from '../components/questions-table'
import QPreview from '../components/questions-preview'
import TypeIn from '../components/type-in'
import { simple } from '@/api/hmmm/subjects' // 学科简单列表
import { creatorssimple } from '@/api/base/users'
import { list } from '@/api/hmmm/directorys' // 目录列表
import { taglist } from '../../api/hmmm/tags' // 标签
import { questionType, difficulty, direction } from '@/api/hmmm/constants' // 第二行需要的接口
import { querestlist } from '../../api/hmmm/questions'
import { citys, provinces } from '@/api/hmmm/citys'
import { choice } from '@/api/hmmm/questions' // 精选题库列表
import QuestionsPreview from '../components/questions-preview.vue'

export default {
  name: 'QusetionChoice',
  components: {
    QTags,
    QTable,
    QPreview,
    TypeIn,
    QuestionsPreview
  },
  data() {
    return {
      value: '',
      subList: [], // 学科列表
      questionType: questionType, // 试题类型
      difficulty: difficulty, // 难度
      direction: direction, // 方向
      tagLists: [], // 标签
      querestlist: [],
      citys: [], // 接收的城区
      provinces: [], // 省份
      activeName: '0',
      basedList: [], // 基础题库列表
      based: '',
      twoSubList: [], // 二级目录
      total: 0, // 数据的总条数
      previewDialog: false, // 控制预览试题对话框的显示与隐藏
      choiceId: 0,
      allQuestion: [], // 全部题库的数据列表
      allState: 0, // 全部题库的总数据
      pendingQuestion: [], //待审核题库的数据列表
      pendingState: 0, // 待审核的总数据
      checkedQuestion: [], //已审核题库的数据列表
      checkedState: 0, // 已审核的总数据
      rejectQuestion: [], //已拒绝题库的数据列表
      rejectState: 0, //已拒绝的总数据
      previewDialog: false, // 控制预览试题对话框的显示与隐藏
      searchForm: {
        subjectID: '', // 学科
        difficulty: '', // 难度
        questionType: '', // 类型
        province: '', //企业省份
        city: '', //企业城市
        keyword: '', // 关键词
        remarks: '', // 题目备注
        shortName: '', // 企业简称
        direction: '', // 方向
        creatorID: '', // 录入人
        catalogID: '', // 目录
        chkState: '',
        tagsValue: '', // 标签
        quest: '', // 题型
        diff: '', //  难度
        dir: '' // 方向
      },
      query: {
        page: 1,
        pagesize: 5
      }
    }
  },
  created() {
    this.getSubList()
    this.tagListChannels()
    this.basisType()
    this.getUserquerestlist()
    this.provinces = provinces()
    this.allQuestions()
    this.getTwoSubList()
  },
  methods: {
    // 获取学科列表
    async getSubList() {
      try {
        const { data } = await simple()
        // console.log(data)
        this.subList = data
        // this.tagListChannels()
        // console.log(this.subList)
      } catch (err) {
        this.$message('获取学科失败！')
      }
    },
    // 学科列表改变事件
    subChange(id) {
      // console.log(id);
      this.getTwoSubList(id)

    },

    // 获取标签列表
    async tagListChannels() {
      try {
        const { data } = await taglist()
        this.tagLists = data.items
        // console.log(this.tagList)
        // console.log('获取标签列表')
      } catch (err) {
        this.$message('获取标签失败！')
      }
    },
    // 获取二级目录
    async getTwoSubList(id) {

      const { data } = await list({
        id: id
      })
      this.twoSubList = data.items
      // console.log(this.twoSubList)
    },
    // 获取录入人
    async getUserquerestlist() {
      try {
        const { data:res } = await creatorssimple()
        this.querestlist = res
      } catch (err) {
        this.$message('获取录入人错误')
      }
    },
    // 获取试题类型
    async basisType() {
      try {
        const { data } = await list()
        this.basedList = data.items
        // console.log(this.basedList)
      } catch (err) {
        this.$message('获取试题类型失败！')
      }
    },
    // 城市 改变事件   获取市区
    async cityChange(city) {
      this.citys = citys(city)
      //  this.searchForm.city = ''
      // let arr = []
      // arr = citys(this.searchForm.province)
      // // console.log(arr)
      // this.citys = arr
    },
    // 清空表单按钮
    clearSearchForm() {
      // console.log(this.$refs.FormRefs)
      this.$refs.FormRefs.resetFields()
    },
    previewQ(item) {
      this.previewDialog = true
      // 将点击的按当前项id 传递过来
      this.cloiceId = item.id
    },
    // tab 栏切换
    handleClick(tab, event) {
      if (tab.name == 1) {
        // 调用 待审核的数据请求
        this.pendingQuestions()
      }
      // console.log(tab, event)
      if (tab.name == 2) {
        // 调用 已审核数据请求
        this.checkedQuestions()
      }
      if (tab.name == 3) {
        // 调用 已拒绝的数据请求
        this.rejectQuestions()
      }
    },
    // 获取当前全部题库
    async allQuestions() {
      const { data } = await choice({
        page: 1,
        pagesize: 5
      })
      // console.log(data)
      this.allQuestion = data.items
      this.allState = data.counts
    },
    // 待审核的题库请求
    async pendingQuestions() {
      const { data } = await choice({
        page: 1,
        pagesize: 5,
        chkState: 0
      })
      // console.log(data);
      this.pendingQuestion = data.items
      this.pendingState = data.counts
    },
    // 已审核的题库请求
    async checkedQuestions() {
      const { data } = await choice({
        page: 1,
        pagesize: 5,
        chkState: 1
      })
      this.checkedQuestion = data.items
      this.checkedState = data.counts
    },
    // 已拒绝的题库请求
    async rejectQuestions() {
      const { data } = await choice({
        page: 1,
        pagesize: 5,
        chkState: 2
      })
      console.log(data)
      this.rejectQuestion = data.items
      this.rejectState = data.counts
    },
    previewQ(item) {
      this.previewDialog = true
      // console.log(item);
      this.choiceId = item.id // 将点击的当前项id传递过来
    }
  }
}
</script>

<style scoped lang="less">
.container {
  .el-icon-edit {
    margin-right: 5px;
  }
  .right-shiti {
    margin-bottom: 15px;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-pack: justify;
    -ms-flex-pack: justify;
    justify-content: space-between;
  }
  .margin-btn {
    float: right;
    padding-left: 65px;
  }
  /deep/ .el-alert__icon.is-big {
    font-size: 21px;
  }
  /deep/ .el-alert .el-alert__description {
    margin: 0 0;
    padding: 0 2px;
  }
}
</style>
