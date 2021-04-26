<template>
  <div class="container">
    <van-tabs v-model="tabs_active" class="width-adapt">
      <van-tab title="主页">
        <van-divider />
        <div>
          <div class="search">
            <van-field v-model="value" label="歌名:" placeholder="歌名" />
            <van-button type="primary" @click="getMusic">搜索</van-button>
          </div>
          <!--列表-->
          <van-cell-group>
            <van-cell
              v-for="(item, i) in song_list"
              :key="i"
              :title="i + 1 + '.' + item.name"
              :value="item.id"
              @click="playMusic(item.id)"
            />
          </van-cell-group>
        </div>
      </van-tab>

      <van-tab title="歌单">
        <orderLists :fatherMethod="playMusic" />
      </van-tab>
      <van-tab title="我的">
        <MyPage />
      </van-tab>
    </van-tabs>

    <!--播放器-->
    <span class="lb-ico toBottom" @click="handlePlayAudio">
      <!-- <img
        :src="
          isPlay
            ? 'https://static.xfanread.com/readingDay2020/quickspot/bf@2x.png'
            : 'https://static.xfanread.com/readingDay2020/quickspot/jy@2x.png'
        "
      /> -->
      <audio ref="audio" :src="audio_src" @ended="audioEnd" controls></audio>
    </span>
  </div>
</template>

<script>
import songs from "../assets/songs.json";
import orderLists from "./orderList/orderList.vue";
import MyPage from "./my/my.vue"
export default {
  components: {
    orderLists,
    MyPage
  },
  data() {
    return {
      playState:{
        currentId:""
      },

      tabs_active: "1",
      isPlay: false,
      value: "前前世世",
      song_list: songs,
      audio_src: "https://music.163.com/song/media/outer/url?id=1315635196"
    };
  },
  mounted(){
   // console.log = function() {}
  },
  methods: {
    // 点击喇叭图标, 开始播放音频
    handlePlayAudio() {
      // console.log(111)
      if (!this.isPlay) {
        this.isPlay = true;
        // 这里使用了audio的原生开始播放事件,同样不加on, 并使用ref获取dom
        this.$refs.audio.play();
      } else {
        this.isPlay = false;
        this.$refs.audio.pause();
      }
    },
    // 音频停止后, 把喇叭置灰
    audioEnd() {
      // console.log(1111)
      this.isPlay = false;
    },
    //play music in lists
    playMusic(id) {
      console.log("click");
      this.audio_src = "https://music.163.com/song/media/outer/url?id=" + id;
      this.isPlay = true;
      this.$refs.audio.pause();
      this.$refs.audio.currentTime = 0.0;
      //暂停后延迟才能播放。
      function sleep(time) {
        return new Promise(resolve => setTimeout(resolve, time));
      }
      sleep(500).then(() => {
        this.$refs.audio.play();
      });
    },
    // get music
    getMusic() {
      let url = "https://musicapi.leanapp.cn/search?keywords=" + this.value;
      this.$axios
        .get(url)
        .then(res => {
          console.log(res.data.result);
          this.song_list = res.data.result.songs;
        })
        .catch(err => {
          console.error(err);
        });
    }
    //end
  }
};
</script>

<style lang="less" scoped>
.container {
  max-width:677px;
  margin: 0px auto;
  padding: 25px;
  /* margin: 0 auto; */
  min-height: 100vh;
  display: flex;
  justify-content: top;
  align-items: top;
  text-align: top;
}
.width-adapt {
  width: 100%;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.search {
  display: flex;
}

.toBottom {
  position: fixed;
  bottom: 0;
  width: 100%;
  z-index: 1;
  max-width:700px;
  margin: 0px auto;
}
</style>
