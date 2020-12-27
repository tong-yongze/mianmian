<template>
  <el-select
    v-model="typeName"
    @change="typeChange"
    placeholder="请选择"
    style="width: 100%"
  >
    <el-option
      v-for="(item, i) in userList"
      :key="i"
      :label="item.username"
      :value="item.username"
    >
    </el-option>
  </el-select>
</template>
<script>
import { simple } from "@/api/base/users";
export default {
  name: "TypeIn",
  components: {},
  props: {
    creatorId: {
      type: String,
      default: "",
    },
  },
  data() {
    return {
      userList: [], // 接受用户信息列表数据
      typeName: "", // 双向绑定录入下拉框数据
    };
  },
  created() {
    this.getUserList();
  },
  methods: {
    async getUserList() {
      const { data } = await simple({
        page: 1,
        pagesize: 10,
        keyword: "",
      });
      // console.log(data);
      this.userList = data;
    },
    // 下拉框选择改变事件
    typeChange(val) {
      // console.log(val);
      this.$emit("update-type-name", val);
    },
  },
};
</script>

<style lang='less' scoped>
</style>