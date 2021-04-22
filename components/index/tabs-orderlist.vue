<template>
  <div>
    <div class="nav">
      <van-field v-model="netease_id" label="网易云ID:" placeholder="ID" />
      <van-icon name="replay" @click="getList" />
    </div>

    <van-grid>
      <van-card
        v-for="(item, i) in playlist"
        :key="i"
        num="1"
        price=""
        :desc="item.id.toString()"
        :title="item.name"
        :thumb="item.coverImgUrl"
        @click="expand_list(item.id.toString())"
      />
    </van-grid>
    <!--遮罩层-->
    <van-overlay :show="show" @click="show = false">
      <div class="wrapper" @click.stop>
        <van-cell-group>
          <van-cell
            v-for="(item, i) in itemInList"
            :key="i"
            :title="item.name"
            :value="item.id"
            @click="click_play(item.id)"
          />
        </van-cell-group>
      </div>
    </van-overlay>
  </div>
</template>

<script>
export default {
  name: "orderLists",
  props: {
    fatherMethod: {
      type: Function,
      default: null
    }
  },
  data() {
    return {
      netease_id: "337139132",
      playlist: [],
      show: false,
      itemInList: []
    };
  },
  mounted() {
    this.getList();
  },
  methods: {
    // 点击遮罩层播放
    click_play(id) {
      if (this.fatherMethod) {
        console.log("click child");
        this.fatherMethod(id)
      }
    },
    //expand
    expand_list(listId) {
      this.show = true;
      let url =
        "https://musicapi.leanapp.cn/playlist/detail?id=" + listId.toString();
      this.$axios
        .get(url)
        .then(res => {
          console.log(res.data);
          this.itemInList = res.data.playlist.tracks;
        })
        .catch(err => {
          console.error(err);
        });
    },
    // get list
    getList() {
      let url =
        "https://musicapi.leanapp.cn/user/playlist?uid=" + this.netease_id;
      this.$axios
        .get(url)
        .then(res => {
          this.playlist = res.data.playlist;
        })
        .catch(err => {
          console.error(err);
        });
    }
  }
};
</script>

<style scoped>
.nav {
  display: flex;
  justify-content: space-between;
}
</style>
