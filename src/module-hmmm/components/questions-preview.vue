<template>
  <div>
    <el-row>
      <el-col :span="6">
        【题型】：
        <span v-if="question.questionType == 1">单选题</span>
        <span v-else-if="question.questionType == 2">多选题</span>
        <span v-else>简答题</span>
      </el-col>
      <el-col :span="6">【编号】：{{ question.id }}</el-col>
      <el-col :span="6">
        【难度】：
        <span v-if="question.difficulty == 1">简单</span>
        <span v-else-if="question.difficulty == 2">一般</span>
        <span v-else>困难</span>
      </el-col>
      <el-col :span="6">【标签】：{{ question.tags }}</el-col>
    </el-row>
    <el-row>
      <el-col :span="6">【学科】：{{ question.subjectName }}</el-col>
      <el-col :span="6">【目录】：{{ question.directoryName }}</el-col>
      <el-col :span="6">【方向】：{{ question.direction }}</el-col>
    </el-row>
    <hr />
    <el-row>【题干】：</el-row>
    <el-row>
      <el-col style="color: blue" v-html="question.question"></el-col>
    </el-row>
    <!-- 题目 -->
    <div>
      <div>
        <span v-if="question.questionType == 1">单选题</span>
        <span v-else-if="question.questionType == 2">多选题</span>
        <span v-else>简答题</span>
        选项: (以下选中的选项为正确答案)
      </div>
      <el-radio-group
        style="display:block"
        v-model="item.isRight"
        v-for="(item, i) in question.options"
        :key="i"
      >
        <el-radio :label="1">{{ item.title }}</el-radio>
      </el-radio-group>
    </div>
    <hr />
    <el-row>
      <el-col>
        【参考答案】：
        <el-button size="mini" type="danger" @click="lookVideo"
          >视频答案预览</el-button
        >
        <div v-show="isShow">
          <video src="https://v-cdn.zjol.com.cn/276984.mp4" class="videoShow"></video>
        </div>
      </el-col>
    </el-row>
    <el-row>
      <el-col> 【答案解析】：<span v-html="question.answer"></span> </el-col>
    </el-row>
    <hr />
    <el-row>
      <el-col> 【题目备注】：{{ question.remarks }} </el-col>
    </el-row>
  </div>
</template>

<script>
import { detail } from '@/api/hmmm/questions'
export default {
  name: 'QPreview',
  props: {
    checkedId: {
      type: Number,
      requried: true
    }
  },
  data() {
    return {
      question: {},
      isShow: false
    }
  },
  created() {
    this.getCheckedQusetion()
  },
  methods: {
    async getCheckedQusetion() {
      const { data } = await detail({
        id: this.checkedId
      })
      console.log(data)
      this.question = data
    },
    lookVideo() {
      this.isShow = !this.isShow
    }
  }
}
</script>

<style scoped lang="less">
.el-col {
  padding: 10px 0;
}
.el-radio-group {
  margin: 15px;
}
/deep/ .videoShow {
  width: 400px;
  height: 300px;
}
</style>
