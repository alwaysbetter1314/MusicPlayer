<template>
<van-skeleton title avatar :row="3" :loading="loading">
  <van-swipe-cell>
    <van-card
      num="2"
      price="2.00"
      desc="描述信息"
      title="商品标题"
      class="goods-card"
      thumb="//img01.yzcdn.cn/vant/cat.jpeg"
    />
    <template #right>
      <van-button square text="删除" type="danger" class="delete-button" />
    </template>
  </van-swipe-cell>
</van-skeleton>
</template>

<script>
export default {
  data() {
    return {
      loading: true,
    };
  },
  mounted() {
    this.loading = false;
  },
};
</script>

<style>
.goods-card {
  margin: 0;
  background-color: white;
}

.delete-button {
  height: 100%;
}
</style>