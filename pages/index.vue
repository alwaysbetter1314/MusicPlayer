<template>
  <div class="container">
    <div>
      <Logo />
      <h1 class="title">
        nuxt-music
      </h1>
      <div class="search">
        <van-field v-model="value" label="歌名:" placeholder="歌名" />
        <van-button type="primary" @click="getMusic">搜索</van-button>
      </div>
      <!--播放器-->
      <span class="lb-ico" @click="handlePlayAudio">
        <img
          :src="
            isPlay
              ? 'https://static.xfanread.com/readingDay2020/quickspot/bf@2x.png'
              : 'https://static.xfanread.com/readingDay2020/quickspot/jy@2x.png'
          "
        />
        <audio ref="audio" :src="audio_src" @ended="audioEnd" controls></audio>
      </span>
      <!--列表-->
      <van-cell-group>
        <van-cell
          v-for="(item, i) in song_list"
          :key="i"
          :title="i + '.' + item.name"
          @click="playMusic(item.id)"
          :value="item.id"
        />
      </van-cell-group>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isPlay: false,
      value: "前前世世",
      song_list: [],
      audio_src: "https://music.163.com/song/media/outer/url?id=1315635196"
    };
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
      this.$refs.audio.currentTime = 0.0;
      this.audio_src = "https://music.163.com/song/media/outer/url?id=" + id;
      this.isPlay = true;
      this.$refs.audio.play();
    },
    // get music
    getMusic() {
      let url = "http://musicapi.leanapp.cn/search?keywords=" + this.value;
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
  }
};
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont,
    "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
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
  justify-content: space-between;
}
</style>

