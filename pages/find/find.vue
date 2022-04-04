<template>
  <view>
    <SearchBar/>
    <Banner/>
    <RecommendBar/>
    <RecommendPlaylist/>
    <newSongs/>
  </view>
</template>

<script>
  import SearchBar from '../../component/Find/SearchBar'
  import Banner from '../../component/Find/Banner'
  import RecommendBar from '../../component/Find/RecommendBar'
  import RecommendPlaylist from "../../component/Find/RecommendPlaylist";
  import newSongs from "../../component/Find/newSongs";

  export default {
    data() {
      return {

      }
    },
    methods: {

    },
    components: {
      SearchBar,
      Banner,
      RecommendBar,
      RecommendPlaylist,
      newSongs
    },

    onLoad() {

    }
  }
</script>

<style>

</style>
