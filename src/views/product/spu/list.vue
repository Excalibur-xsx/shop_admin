<template>
  <div>
    <SkuList v-if="isShowSkuList" :spuItem="spuItem" @showList="showList" />

    <div v-else>
      <Category :disabled="!isShowList" />
      <SpuShowList v-if="isShowList" @showUpdateList="showUpdateList" @showSpuList="showSpuList" />
      <SpuUpdateList v-else :item="item" @showList="showList" />
    </div>
  </div>
</template>

<script>
import Category from "@/components/Category";
import SpuShowList from "./spuShowList";
import SpuUpdateList from "./spuUpdateList";
import SkuList from "./skuList";

export default {
  name: "SpuList",
  data() {
    return {
      isShowList: true,
      item: {},
      isShowSkuList: false,
      spuItem: {},
    };
  },
  methods: {
    showSpuList(row) {
      this.isShowSkuList = true;
      this.spuItem = { ...row };
    },
    showUpdateList(row) {
      this.isShowList = false;
      this.item = { ...row };
    },
    showList(category) {
      this.isShowList = true;
      this.isShowSkuList = false;
    },
  },
  beforeDestroy() {
    this.$store.commit("category/RESET_CATEGORY_ID");
  },
  components: {
    Category,
    SpuShowList,
    SpuUpdateList,
    SkuList,
  },
};
</script>
