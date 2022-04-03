<template>
  <div class="recommend_bar_wrapper">
    <ul class="recommend_list_wrapper">
      <li class="recommend_item"
          v-for="item in itemData" :key="item.id">
        <img :src="item.imgUrl" alt="">
        <div class="item_text">{{item.text}}</div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "RecommendBar",
  data() {
    return {
      itemData: [
        {id: 1, imgUrl: `static/Find/RecommendBar/find_calender.png`, text: `每日推荐`},
        {id: 2, imgUrl: `static/Find/RecommendBar/fm.png`, text: `私人FM`},
        {id: 3, imgUrl: `static/Find/RecommendBar/find_playlist.png`, text:`歌单™`},
        {id: 4, imgUrl: `static/Find/RecommendBar/find_rank.png`,text: `排行榜`},
        {id: 5, imgUrl: `static/Find/RecommendBar/find_stream.png`, text: `直播`},
        {id: 6, imgUrl: `static/Find/RecommendBar/find_album.png`, text: `数字专辑`},
        {id: 7, imgUrl: `static/Find/RecommendBar/find_think.png`, text:`专注冥想`},
        {id: 8, imgUrl: `static/Find/RecommendBar/find_song_home.png`, text: `歌房`},
        {id: 9, imgUrl: `static/Find/RecommendBar/find_game.png`, text: `游戏专区`},
      ]
    }
  }
}
</script>

<style scoped lang="less">
.recommend_bar_wrapper {
  width: 100vw;
  height: 20vw;
  overflow-x: scroll;
  overflow-y: hidden;
  margin-top: 3vw;

  .recommend_list_wrapper {
    display: flex;
    flex-direction: row;
    align-items: center;
    padding-left: 0;

    .recommend_item {
      width: 18vw;
      height: 18vw;
      display: flex;
      flex-direction: column;
      justify-content: center;
      margin-left: 3vw;
      margin-right: 3vw;

      img {
        width: 18vw;
        height: 10vw;
      }

      .item_text {
        text-align: center;
        font-size: 4vw;
        font-weight: bold;
      }
    }
  }
}
</style>
