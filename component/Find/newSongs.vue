<template>
  <div class="new_songs_wrapper">
    <div class="all_new_title_bar">
      <span class="new_song_title">新歌速递</span>
      <div class="play_button">播放</div>
    </div>
      <ul class="playlist_outer_wrapper">
        <li class="playlist_outer_item"
            v-for="item in newSongsList" :key="item.id">
          <div class="song_img" :style="{backgroundImage: `url(${item.picUrl})`}">
            <img src="static/Find/find_play_white.png" alt="播放">
          </div>
          <div class="song_title">
            <div class="song_big_title">{{item.name}}</div>
            -<span class="authors" v-for="author in item.song.artists">{{author.name}} </span>
          </div>
        </li>
      </ul>
  </div>
</template>

<script>
export default {
  name: "newSongs",
  data() {
    return {
      newSongsList: []
    }
  },
  methods: {
    /**
     * 获取新歌列表
     * */
    async getNewSongsList() {
      await uni.request({
        url: `${this.$store.state.localhost}/personalized/newsong`,
        method: `get`,
        data: {
          limit: 12
        },
        success: (res) => {
          // 输出请求的新歌的数组数据
          // console.log(res.data);
          this.newSongsList = res.data.result
          console.log(this.newSongsList = res.data.result);
        }
      })
    }
  },
  async beforeMount() {
    await this.getNewSongsList()
  }
}
</script>

<style scoped lang="less">

</style>
