
<template>
  <div class="pagination">
    <div>共{{ totalNum }}条</div>
    <el-select
      v-model="value"
      placeholder="请选择"
      @change="handleChange"
      size="mini"
    >
      <el-option
        v-for="item in [10, 20, 50, 100]"
        :key="item"
        :label="`每页${item}条`"
        :value="item"
      >
      </el-option>
    </el-select>
    <count-total
      ref="elPag"
      :startPage="startPage"
      :currentPage="currentPage"
      :pagerCount="pagerCount"
      :groupCount="groupCount"
      :disabled="false"
      @change="pageChange"
    >
    </count-total>
    <div>当前第{{ currentGroup + 1 }}组</div>
    <el-button type="primary" @click="changeGroup(-1)">上一组</el-button>
    <el-button type="primary" @click="changeGroup(1)">下一组</el-button>
  </div>
</template>

<script>
import CountTotal from "./CountTotal";
export default {
  components: {
    CountTotal,
  },
  data() {
    return {
      currentPage: 1, //当前页
      totalNum: 232, //总条数
      value: 10, //一页多少条
      groupNum: 100, //一组总条数
      currentGroup: 0, //组数
      pagerCount: 7, //组件显示几页
    };
  },
  computed: {
    totalGroup() {
      return Math.ceil(this.totalNum / this.groupNum);
    },
    groupCount() {
      return Math.ceil(this.groupNum / this.value);
    },
    startPage() {
      return this.groupCount * this.currentGroup + 1;
    },
  },
  methods: {
    changeGroup(value) {
      if (value === -1) {
        if (this.currentGroup == 0) {
          this.$message({ type: "warning", message: "当前组是第一组" });
          return;
        }
        this.currentGroup--;
      } else if (value === 1) {
        if (this.totalGroup < this.currentGroup + 2) {
          this.$message({ type: "warning", message: "当前组是最后一组" });
          return;
        }
        this.currentGroup++;
      }
      this.currentPage = this.currentGroup * this.groupCount + 1;
    },
    handleChange(val) {
      console.log(`每页 ${val} 条`);
      this.currentGroup = 0;
      this.currentPage = 1;
    },
    pageChange(newPage) {
      if (newPage === this.currentGroup * this.groupCount) {
        newPage++;
      }
      this.currentPage = newPage;
    },
  },
};
</script>

<style lang="scss" scoped>
</style>
