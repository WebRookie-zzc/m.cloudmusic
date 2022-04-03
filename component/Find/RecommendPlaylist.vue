<template>
  <div class="recommend_playlist_wrapper">
    <div class="top_title">
      <span class="title_text">推荐歌单</span>
      <div class="more_button">更多 &gt;</div>
    </div>
    <ul class="playlist_item_wrapper">
<!--      滚动显示的歌单-->
      <li class="playlist_scroll_wrapper">
        <ul class="playlist_scroll_inner_wrapper">
          <li class="scroll_playlist_item" v-for="scrollItem in scrollPlaylistList">
            <img :src="scrollItem.picUrl" alt="">
            <div class="playlist_title">{{scrollItem.name}}</div>
          </li>
        </ul>
      </li>
<!--      静态显示的歌单-->
      <li class="playlist_item" v-for="item in staticPlaylistList">
        <img :src="item.picUrl" alt="">
        <div class="playlist_title">{{item.name}}</div>
        <div class="play_count">{{playCounter(item.playCount)}}</div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "RecommendPlaylist",
  data () {
    return {
      //请求到的所有推荐歌单的数据
      recommendPlaylistList: [],
      // 前面用于滚动的歌单的数据
      scrollPlaylistList: [],
      // 用于静态显示的歌单数据
      staticPlaylistList: []
    }
  },
  methods: {
    /**
     * 获取未登录时的推荐歌单
     * */
    async getRecommendPlaylistData() {
      await uni.request({
        url: `${this.$store.state.localhost}/personalized`,
        data: {
          limit: 10
        },
        success: (res) => {
          //输出推荐歌单列表的数据(未登录时的数据)
          // console.log(res.data.result);
          this.recommendPlaylistList = res.data.result
          this.scrollPlaylistList  = this.recommendPlaylistList.slice(0,5)
          this.staticPlaylistList = this.recommendPlaylistList.slice(5, this.recommendPlaylistList.length + 1)
        }
      })
    },
    /**
     * 处理播放量数据
     * @param originData {string} 播放量原始数据
     * */
    playCounter(originData) {
      // 换为以万为单位
      return `${parseInt((Number(originData) / 1000).toString())}万`
    }
  },

  async beforeMount () {
    await this.getRecommendPlaylistData()
  }
}
</script>

<style scoped lang="less">
  .recommend_playlist_wrapper {
    width: 100vw;
    margin-top: 3vw;
    border-top: 1px solid #aaa;
    padding-top: 2vw;

    .top_title {
      display: flex;
      flex-direction: row;
      justify-content: space-between;

      span {
        font-weight: bolder;
        font-size: 5vw;
        margin-left: 3vw;
        line-height: 5vw;
      }

      .more_button {
        margin-right: 5vw;
        width: 13vw;
        height: 5vw;
        padding-right: 1.5vw;
        padding-left: 1.5vw;
        line-height: 5vw;
        font-size: 3.3vw;
        border: 1px solid #ccc;
        border-radius: 3vw;
        text-align: center;
      }
    }

    .playlist_item_wrapper {
      list-style: none;
      margin-top: 3vw;
      padding: 0;
      display: flex;
      flex-direction: row;
      flex-wrap: nowrap;
      width: 100vw;
      overflow-x: scroll;

      .playlist_scroll_wrapper {
        width: 25vw;
        height: 20vw;
        list-style: none;
      }

      .playlist_item {
        width: 25vw;
        height: 30vw;
        margin-left: 2vw;
        margin-right: 2vw;
        position: relative;

        img {
          width: 25vw;
          height: 25vw;
          border-radius: 5vw;
        }

        .playlist_title {
          margin-top: 1vw;
          width: 23vw;
          font-size: 2.5vw;
          padding-left: 1vw;
          padding-rignt: 1vw;
          font-weight: bolder;
          text-overflow: ellipsis;
          white-space: nowrap;
          overflow: hidden;
        }

        .play_count {
          position: absolute;
          width: 12vw;
          height: 5vw;
          right: 1vw;
          top: 2vw;
          color: #fff;
          padding-left: 5vw;
          background: rgba(0, 0, 0, .5) url("/static/Find/play_count.png") 2vw center no-repeat;
          background-size: 3vw 3vw;
          border-radius: 3vw;
          text-align: center;
        }
      }
    }
  }
</style>
