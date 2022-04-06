<template>
  <div class="new_songs_wrapper">
    <div class="all_new_title_bar">
      <span class="new_song_title">新歌速递</span>
      <div class="play_button">播放</div>
    </div>
      <ul class="playlist_outer_wrapper"
          :style="{left: `-${innerWrapperLeftValue}vw `, transition: transitionValue}"
          @touchstart="handleTouchStart"
          @touchmove="handleTouchMove"
          @touchend="handleTouchEnd">
        <li class="playlist_outer_item"
            v-for="item in newSongsList" :key="item.id">
          <div class="song_img" :style="{backgroundImage: `url(${item.picUrl})`}">
            <img src="static/Find/find_play_white.png" alt="播放">
          </div>
          <div class="song_title">
            <div class="song_big_title">{{item.name}}</div>
            <div class="song_all_authors">
              <span class="authors" v-for="author in item.song.artists">{{author.name}}&nbsp;</span>
            </div>
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
      // 请求来的新歌数据
      newSongsList: [],
      // 当前新歌所处的页数
      currentIndex: 0,
      // 视口的宽度
      screenWidth: 0,
      // 手指按下时的位置
      startTouchPositionX: 0,
      // 滚动元素的 left 值
      innerWrapperLeftValue: 0,
      // 手指触摸时的left值
      startTouchInnerWrapperLeftValue : 0,
      // 下一页或上一页 0 表示不跳转 1 表示下一页 -1 表示上一页
      nextOrLastPage: 0,
      // 滑动 transition 值
      transitionValue: `none`
    }
  },
  methods: {
    /**
     * 获取视口的宽度
     * */
    async getViewportWidth() {
      await uni.getSystemInfo({
        success: (res) => {
          this.screenWidth = res.screenWidth;
          //输出获取到的视口宽度
          // console.log(res.screenWidth);
        }
      });
    },
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
          // console.log(this.newSongsList = res.data.result);
        }
      })
    },

    /**
     * 处理 touchStart事件
     * */
    handleTouchStart(event) {
      this.startTouchPositionX = event.touches[0].pageX
      this.startTouchInnerWrapperLeftValue = this.innerWrapperLeftValue
      this.nextOrLastPage = 0
    },
    /**
     * 处理 touchmove 事件
     * */
    handleTouchMove(event) {
      let fingerPositionPx = event.touches[0].pageX
      // console.log(fingerPositionPx);
      // console.log(this.screenWidth);
      let movePositionVw = ( fingerPositionPx - this.startTouchPositionX ) / this.screenWidth * 100
      // 根据手指划过的距离来判断是否进行页面的滑动切换
      if(movePositionVw > 20) this.nextOrLastPage = -1
      else if(movePositionVw < -20) this.nextOrLastPage = 1
      else this.nextOrFirstPage = 0
      // console.log(movePositionVw);
      if(this.startTouchInnerWrapperLeftValue - movePositionVw < 0) this.innerWrapperLeftValue = 0
      else if(this.startTouchInnerWrapperLeftValue - movePositionVw > 270) this.innerWrapperLeftValue = 270
      else this.innerWrapperLeftValue = this.startTouchInnerWrapperLeftValue - movePositionVw
      // console.log(this.innerWrapperLeftValue);
    },
    /**
     * 处理 touchend事件
     **/
    handleTouchEnd(event) {
      this.transitionValue = `.5s`
      console.log(this.currentIndex);
      if(this.nextOrLastPage === 0) {}
      else if(this.nextOrLastPage === 1) {
        this.currentIndex += 1
        if(this.currentIndex > 3) this.currentIndex = 3
      }else if(this.nextOrLastPage === -1) {
        this.currentIndex -= 1
        if(this.currentIndex < 0) this.currentIndex = 0
      }
      this.innerWrapperLeftValue = 90 * this.currentIndex
      setTimeout(() => {this.transitionValue = `none`}, 500);
    }
  },
  async beforeMount() {
    await this.getNewSongsList()
  },
  async mounted() {
    await this.getViewportWidth()
  }
}
</script>

<style scoped lang="less">
  .new_songs_wrapper {
    width: 100vw;
    height: 70vw;
    overflow: hidden;
    margin-top: 2vw;
    border-top: 1px solid #ccc;

    .all_new_title_bar {
      width: 100vw;
      height: 10vw;
      display: flex;
      justify-content: space-between;
      align-items: center;

      .new_song_title {
        font-size: 5vw;
        font-weight: bolder;
        margin-left: 3vw;
      }

      .play_button{
        width: 12vw;
        height: 5vw;
        border: 1px solid #ccc;
        font-size: 3vw;
        line-height: 5vw;
        border-radius: 3vw;
        padding-left: 5vw;
        text-align: center;
        color: #fff;
        background: rgba(0, 0, 0, .5) url(../../static/Find/find_play_white.png) no-repeat 2vw center;
        background-size: 3vw 3vw;
        margin-right: 3vw;
      }
    }

    .playlist_outer_wrapper {
      list-style: none;
      padding: 0;
      display: flex;
      flex-direction: column;
      flex-wrap: wrap;
      width: 300vw;
      height: 60vw;
      position: relative;

      .playlist_outer_item {
        width: 80vw;
        height: 20vw;
        margin-left: 5vw;
        margin-right: 5vw;
        display: flex;
        flex-direction: row;

        .song_img {
          width: 18vw;
          height: 18vw;
          border-radius: 3vw;
          background-size: 18vw 18vw;
          display: flex;
          justify-content: center;
          align-items: center;

          img {
            width: 7vw;
            height: 7vw;
          }
        }

        .song_title {
          width: 55vw;
          height: 20vw;
          margin-left: 7vw;
          display: flex;
          flex-direction: column;
          justify-content: space-around;

          .song_big_title {
            font-size: 4vw;
            font-weight: bolder;
          }

          .song_all_authors {
            color: #777;
            font-size: 3vw;
          }
        }
      }
    }
  }
</style>
