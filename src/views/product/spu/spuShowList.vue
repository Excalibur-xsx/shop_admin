<template>
  <el-card style="margin-top: 20px">
    <el-button
      type="primary"
      icon="el-icon-plus"
      :disabled="!category.category3Id"
      @click="$emit('showUpdateList', { category3Id: category.category3Id })"
    >添加SPU</el-button>

    <el-table :data="spuList" v-loading="loading" border style="width: 100%; margin: 20px 0">
      <el-table-column type="index" label="序号" width="80" align="center"></el-table-column>
      <el-table-column prop="spuName" label="SPU名称"></el-table-column>
      <el-table-column prop="description" label="SPU描述"></el-table-column>
      <el-table-column label="操作">
        <template slot-scope="{ row }">
          <el-button
            type="primary"
            icon="el-icon-plus"
            size="mini"
            @click="$emit('showSpuList', { ...row, ...category })"
          ></el-button>
          <el-button
            type="primary"
            icon="el-icon-edit"
            size="mini"
            @click="$emit('showUpdateList', { ...row, ...category })"
          ></el-button>
          <el-button type="info" icon="el-icon-info" size="mini"></el-button>
          <el-button type="danger" icon="el-icon-delete" size="mini"></el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination
      class="pagination"
      @size-change="getPageList(page, $event)"
      @current-change="getPageList($event, limit)"
      :page-sizes="[3, 6, 9]"
      :page-size.sync="limit"
      :current-page="page"
      layout="prev, pager, next, jumper, sizes, total"
      :total="total"
    ></el-pagination>
  </el-card>
</template>

<script>
import { mapState } from "vuex";

export default {
  name: "SpuShowList",
  data() {
    return {
      page: 1,
      limit: 3,
      total: 0,
      spuList: [],
      loading: false,
    };
  },
  computed: {
    ...mapState({
      category: (state) => state.category.category,
    }),
  },
  watch: {
    "category.category3Id": {
      handler(category3Id) {
        if (!category3Id) return;
        this.getPageList(this.page, this.limit);
      },
      immediate: true, // 一上来触发一次
    },
    "category.category1Id"() {
      this.clearList();
    },
    "category.category2Id"() {
      this.clearList();
    },
  },
  methods: {
    // 获取SPU分页列表
    async getPageList(page, limit) {
      this.loading = true;
      const { category3Id } = this.category;
      const result = await this.$API.spu.getSpuList({
        page,
        limit,
        category3Id,
      });
      if (result.code === 200) {
        this.$message.success("获取SPU分页列表成功~");
        this.spuList = result.data.records;
        this.total = result.data.total;
      } else {
        this.$message.error(result.message);
      }
      this.loading = false;
    },
    clearList() {
      this.spuList = [];
      this.page = 1;
      this.limit = 3;
      this.total = 0;
    },
  },
};
</script>

<style>
</style>
