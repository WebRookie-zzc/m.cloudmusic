<template>
  <div class="search_bar">
<!--    左侧才暗淡图标-->
    <div class="left_menu"></div>
<!--    中间搜索框-->
    <div class="input_search_wrapper">
      <div class="left_search_img"></div>
      <input type="text" placeholder="搜索">
    </div>
<!--    右侧听歌识曲-->
    <div class="mic"></div>
  </div>
</template>

<script>
  export default {
    name: 'SearchBar',
    data() {
      return {

      }
    },
    methods: {

    }
  }
</script>

<style lang="less">
  .search_bar {
    width: 100vw;
    height: 10vw;
    //border: 1px solid #000;
    display: flex;
    align-items: center;

    .left_menu {
      width: 10vw;
      height: 10vw;
      background: url("/static/Find/menu.png") center center no-repeat;
      background-size: 8vw 8vw;
    }

    .input_search_wrapper {
      width: 65vw;
      margin-left: 5vw;
      margin-right: 5vw;
      height: 9vw;
      border-radius: 10vw;
      border: 1px solid #000;
      padding-left: 2vw;
      display: flex;

      .left_search_img {
        width: 9vw;
        height: 9vw;
        background: url("/static/Find/search.png") center center no-repeat;
        background-size: 8vw 8vw;
      }

      input {
        height: 100%;
        width: 50vw;
      }
    }

    .mic {
      width: 10vw;
      height: 10vw;
      background: url("/static/Find/mic.png") center center no-repeat;
      background-size: 8vw 8vw;
    }
  }
</style>
