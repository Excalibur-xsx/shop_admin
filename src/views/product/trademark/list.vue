<template>
  <div>
    <el-button type="primary" icon="el-icon-plus">添加</el-button>

    <el-table :data="trademarkList" border style="width: 100%; margin: 20px 0">
      <el-table-column type="index" label="序号" width="80" align="center"></el-table-column>
      <el-table-column prop="tmName" label="品牌名称"></el-table-column>
      <el-table-column label="品牌LOGO">
        <template slot-scope="scope">
          <img class="trademark-img" :src="scope.row.logoUrl" alt="logo" />
        </template>
      </el-table-column>
      <el-table-column label="操作">
        <template>
          <el-button type="warning" icon="el-icon-edit">修改</el-button>
          <el-button type="danger" icon="el-icon-delete">删除</el-button>
        </template>
      </el-table-column>
    </el-table>

    <el-pagination
      class="trademark-pagination"
      @size-change="getPageList(page, $event)"
      @current-change="getPageList($event, limit)"
      :page-sizes="[3, 6, 9]"
      :page-size.sync="limit"
      :current-page.sync="page"
      layout="prev, pager, next, jumper, sizes, total"
      :total="total"
    ></el-pagination>
  </div>
</template>

<script>
// import { trademark } from "@/api";

export default {
  name: "TrademarkList",
  data() {
    return {
      trademarkList: [],
      total: 0,
      page: 1,
      limit: 3,
    };
  },
  methods: {
    // handleSizeChange(limit) {
    //   this.getPageList(this.page, limit);
    // },
    // handleCurrentChange(page) {
    //   this.getPageList(page, this.limit);
    // },
    //请求分页列表数据
    async getPageList(page, limit) {
      try {
        const result = await this.$API.trademark.getPageList(page, limit);
        // console.log(result);
        if (result.code === 200) {
          this.$message.success("获取品牌分页列表数据成功！");
          this.trademarkList = result.data.records;
          this.total = result.data.total;
          this.page = result.data.current; //当前页码
          this.limit = result.data.size; //当前条数
        } else {
          this.$message.error("获取品牌分页列表数据失败！");
        }
      } catch (error) {
        this.$message.error("获取品牌分页列表数据失败！");
        console.log("error" + error);
      }
    },
  },
  mounted() {
    //#region
    //   try {
    //     const result = await this.$API.trademark.getPageList(1, 3);
    //     // console.log(result);
    //     if (result.code === 200) {
    //       this.$message.success("获取品牌分页列表数据成功！");
    //       this.trademarkList = result.data.records;
    //       this.total = result.data.total;
    //     } else {
    //       this.$message.error("获取品牌分页列表数据失败！");
    //     }
    //   } catch (error) {
    //     this.$message.error("获取品牌分页列表数据失败！");
    //     console.log("error" + error);
    //   }
    //#endregion
    this.getPageList(this.page, this.limit);
  },
};
</script>

<style lang="sass" scoped>
.trademark-img
  width: 150px
.trademark-pagination
  text-align: right
>>>.el-pagination__sizes
  margin-left: 250px
</style>
