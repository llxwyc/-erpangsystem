<template>
  <div class="block">
    <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="+pagedata.currentPage"
      :page-sizes="[5, 8, 10, 15]"
      :page-size="+pagedata.pageSize"
      layout="total, sizes, prev, pager, next, jumper"
      :total="+datatotal"
    >
    </el-pagination>
  </div>
</template>

<script>

import { createNamespacedHelpers } from "vuex";
const { mapActions, mapState ,mapMutations} = createNamespacedHelpers("Admin");

export default {
  computed:{
...mapState(["datatotal", "pagetotal", "pagedata"]),
  },
  methods: {
    ...mapMutations(['changePageSize','changeCurrentPage']),
    ...mapActions(["getAdmins"]),

    handleSizeChange(val) {
      this.changePageSize(val);
      this.getAdmins();
    },
    handleCurrentChange(val) {
      this.changeCurrentPage(val);
      this.getAdmins();
    },
  },
};
</script>

<style>
  .block {
    text-align: center;
    margin-top: 10px;
  }
</style>