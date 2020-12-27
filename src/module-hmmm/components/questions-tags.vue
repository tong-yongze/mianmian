<template>
  <el-select
    v-model="tagName"
    placeholder="请选择"
    size="small"
    style="width: 100%"
    @change="tagChange"
  >
    <el-option
      v-for="item in tagList"
      :key="item.id"
      :label="item.tagName"
      :value="item.tagName"
    >
    </el-option>
  </el-select>
</template>
<script>
import { list } from "@/api/hmmm/tags";
export default {
  name: "QTags",
  components: {},
  props: {
    subjectId: {
      type: [Number, String],
      default: "",
    },
    tags: {
      type: String,
      default: "",
    },
  },
  data() {
    return {
      tagList: [],
      tagName: this.tags,
    };
  },
  watch: {},
  watch: {
    subjectId(val) {
      this.getTagList(val);
    },
  },
  created() {},
  methods: {
    async getTagList(val) {
      const { data } = await list({
        subjectID: val,
      });
      // console.log(data);
      this.tagList = data.items;
    },
    tagChange(name) {
      this.$emit("input", name);
    },
  },
};
</script>

<style lang='scss' scoped>
</style>