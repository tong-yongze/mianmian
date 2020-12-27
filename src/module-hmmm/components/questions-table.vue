<template>
  <div>
    <el-alert
      :title="`数据一共${conuts}条`"
      type="info"
      show-icon
      :closable="false"
    >
    </el-alert>
    <el-table :data="questionBank" style="width: 1880px">
      <el-table-column prop="number" label="试题编号"></el-table-column>
      <el-table-column prop="subject" label="学科"></el-table-column>
      <el-table-column prop="catalog" label="目录"></el-table-column>
      <el-table-column label="题型">
        <template slot-scope="scope">
          <div v-if="scope.row.questionType == 1">单选</div>
          <div v-else-if="scope.row.questionType == 2">多选</div>
          <div v-else>简答</div>
        </template>
      </el-table-column>
      <el-table-column label="题干">
        <template slot-scope="scope">
          <div v-html="scope.row.question"></div>
        </template>
      </el-table-column>
      <el-table-column label="录入事件">
        <template slot-scope="scope">
          {{ scope.row.addDate | parseTimeByString }}
        </template>
      </el-table-column>
      <el-table-column label="难度">
        <template slot-scope="scope">
          <div v-if="scope.row.difficulty == 1">简单</div>
          <div v-else-if="scope.row.difficulty == 2">一般</div>
          <div v-else>困难</div>
        </template>
      </el-table-column>
      <el-table-column prop="creator" label="录入人"></el-table-column>
      <el-table-column label="审核状态">
        <template slot-scope="scope">
          <div v-if="scope.row.chkState == 0">待审核</div>
          <div v-else-if="scope.row.chkState == 1">已审核</div>
          <div v-else>已拒绝</div>
        </template>
      </el-table-column>
      <el-table-column prop="chkRemarks" label="审核意见"></el-table-column>
      <el-table-column label="发布状态">
        <template slot-scope="scope">
          <div v-if="scope.row.publishState == 0 && scope.row.chkState == 1">
            已下架
          </div>
          <div v-else-if="scope.row.publishState == 1">已发布</div>
          <div v-else>待发布</div>
        </template>
      </el-table-column>
      <el-table-column label="操作" fixed="right">
        <template slot-scope="scope">
          <el-button
            type="text"
            size="mini"
            @click="$emit('question-preview', scope.row)"
            >预览</el-button
          >
          <el-button
            type="text"
            size="mini"
            :disabled="
              scope.row.chkState == 1 || scope.row.chkState == 2 ? flag : err
            "
            >审核</el-button
          >
          <el-button
            @click="$emit('edit-question')"
            type="text"
            size="mini"
            :disabled="
              (scope.row.publishState !== 1 && scope.row.chkState !== 1) ||
              scope.row.publishState == 0
                ? err
                : flag
            "
            >修改</el-button
          >
          <el-button type="text" size="mini">{{
            scope.row.chkState == 1 && scope.row.publishState !== 1
              ? "上架"
              : "下架"
          }}</el-button>
          <el-button
            type="text"
            size="mini"
            :disabled="
              scope.row.chkState == 1 && scope.row.publishState == 1
                ? flag
                : err
            "
            >删除</el-button
          >
        </template>
      </el-table-column>
    </el-table>

    <!-- 分页 -->
    <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="query.page"
      :page-sizes="[3, 10, 20, 30]"
      :page-size="query.pagesize"
      layout="->, total, sizes, prev, pager, next, jumper"
      :total="conuts"
      background
    >
    </el-pagination>
  </div>
</template>
<script>
import { parseTimeByString } from "@/filters";
export default {
  name: "QTable",
  components: {},
  props: {
    questionBank: {
      type: Array,
      default: [],
    },
    conuts: {
      type: Number,
      default: 0,
    },
    query: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      flag: true,
      err: false,
    };
  },
  created() {},
  methods: {
    // 监听 显示条数
    handleSizeChange(newSize) {
      this.query.pagesize = newSize;
      this.$emit("update-size");
    },
    // 监听 页码值
    handleCurrentChange(newPage) {
      console.log(newPage);
      this.query.page = newPage;
      this.$emit("update-page");
    },
  },
};
</script>

<style lang='scss' scoped>
.el-alert {
  margin-bottom: 15px;
}
.el-pagination {
  margin: 15px;
}
</style>