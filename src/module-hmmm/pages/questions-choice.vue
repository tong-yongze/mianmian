<template>
  <div class="container">
    <el-card class="box-card">
      <div class="right-shiti">
        <span>说明</span>
        <el-button
          type="button"
          class="el-button el-button--success el-button--small"
          size="small"
        >
          <i class="el-icon-edit"></i>
          <span>新增试题</span>
        </el-button>
      </div>

      <!-- 第一行区域 -->
      <el-form
      ref="FormRefs"
      label-width="80px"
      :model="searchForm"
      >
        <el-row>
          <!-- 学科 -->
          <el-col :span="6">
            <el-form-item
            prop="subjectID"
            label="学科"
            size="small"
            >
              <el-select
              v-model="searchForm.subjectID"
              placeholder="请选择"
              @change="subChange"
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
            <el-form-item prop="catalogID" label="二级目录" label-width="80px" size="small">
              <el-select v-model="searchForm.catalogID" placeholder="请选择">
                <el-option
                  v-for="item in twoSubList"
                  :key="item.id"
                  :label="item.directoryName"
                  :value="item.id"
                >
                </el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <!-- 标签 -->
          <el-col :span="6">
            <el-form-item label="标签" label-width="80px" size="small">
              <el-select v-model="searchForm.tagsValue" placeholder="请选择">
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
            <el-form-item  label="关键词" label-width="80px">
              <el-input  size="small"></el-input>
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
              <el-select
              v-model="searchForm.quest"
              placeholder="请选择"
              >
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
              <el-select v-model="searchForm.diff" placeholder="请选择">
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
            <el-form-item prop="direction" label="方向" size="small" label-width="80px">
              <el-select v-model="searchForm.dir" placeholder="请选择">
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
            prop="creators"
            label="录入人"
            size="small"
            label-width="80px"
            >
              <el-select v-model="searchForm.creators">
                <el-option
                v-for="creatorItem in querestlist"
                    :key="creatorItem.id"
                    :label="creatorItem.creator"
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
            <el-form-item prop="remarks" label="题目备注" label-width="80px" size="small">
              <el-input v-model="searchForm.remarks" ></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="6">
            <!-- 企业简称 -->
            <el-form-item prop="shortName" label="企业简称" label-width="80px" size="small">
              <el-input v-model="searchForm.shortName" ></el-input>
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
                      v-for="(item, i) in provinces"
                      :key="i"
                      :label="item"
                      :value="item"
                    >
                    </el-option>
                  </el-select>
                  <el-select
                    v-model="searchForm.city"
                    placeholder="请选择"
                    size="small"
                    style="width: 50%"
                     @change="cityChange"
                  >
                    <el-option
                      v-for="(item, i) in citys"
                      :key="i"
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

      <el-tabs v-model="activeName" type="card" @tab-click="handleClick">
          <el-tab-pane label="用户管理" name="first">用户管理</el-tab-pane>
          <el-tab-pane label="配置管理" name="second">配置管理</el-tab-pane>
          <el-tab-pane label="角色管理" name="third">角色管理</el-tab-pane>
          <el-tab-pane label="定时任务补偿" name="fourth">定时任务补偿</el-tab-pane>
      </el-tabs>

    </el-card>
  </div>
</template>

<script>
import { simple } from "@/api/hmmm/subjects"  // 学科简单列表
import { creatorssimple } from '../../api/base/users'
import { list } from "@/api/hmmm/directorys"  // 目录列表
import { taglist } from '@/api/hmmm/tags'  // 标签
import { questionType, difficulty, direction } from "@/api/hmmm/constants" // 第二行需要的接口
import { querestlist } from '../../api/hmmm/questions'
import { citys, provinces } from "@/api/hmmm/citys"
export default {
  name: "QusetionChoice",
  data() {
    return {
      value: '',
      subList: [], // 学科列表
      questionType: questionType, // 试题类型
      difficulty: difficulty, // 难度
      direction: direction ,// 方向
      tagLists: [],  // 标签
      creators:'', //  录入人
      querestlist: [],
      citys: [], // 接收的城区
      provinces: [], // 省份
      activeName: 'all',
      basedList: [], // 基础题库列表
      based: '',
      twoSubList: [] , // 二级目录
      total: 0 ,// 数据的总条数

      searchForm: {
        subjectID: "", // 学科
        difficulty: "", // 难度
        questionType: "", // 类型
        province: "", //企业省份
        city: "", //企业城市
        keyword: "", // 关键词
        remarks: "", // 题目备注
        shortName: "", // 企业简称
        direction: "", // 方向
        creatorID: "", // 录入人
        catalogID: "", // 目录
        chkState: "",
        tagsValue: '', // 标签
        quest:'', // 题型
        diff:'' , //  难度
        dir: '' // 方向
      }
    }
  },
  created() {
    this.getSubList()
    this.tagListChannels()
    this.basisType()
    this.getUserquerestlist()
    this.provinces = provinces()
    // this.getTagsList()
  },
  methods: {
    // 获取学科列表
    async getSubList() {
      try {
        const { data } = await simple()
        // console.log(data)
        this.subList = data
        this.tagListChannels()
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
        // console.log(data)
        this.tagLists = data.items
        // console.log(this.tagList)
      } catch (err) {
        this.$message('获取标签失败！')
      }
    },
  // 获取二级目录
    async getTwoSubList(id) {
      const { data } =  await list({
        id: id
      })
      // console.log(data)
      this.twoSubList = data.items

    },
    // 获取录入人
    async getUserquerestlist() {
      try {
        const { data } = await querestlist()
        // console.log(data)
        this.querestlist = data.items
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
      this.citys  = citys(city)
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
    // tab 栏切换
    handleClick(tab, event) {
      // console.log(tab, event)
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
