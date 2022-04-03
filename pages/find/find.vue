<template>
  <view>
    <SearchBar/>
    <Banner/>
    <RecommendBar/>
    <RecommendPlaylist/>
  </view>
</template>

<script>
  import SearchBar from '../../component/Find/SearchBar'
  import Banner from '../../component/Find/Banner'
  import RecommendBar from '../../component/Find/RecommendBar'
  import RecommendPlaylist from "../../component/Find/RecommendPlaylist";

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
      RecommendPlaylist
    },

    onLoad() {

    }
  }
</script>

<style>

</style>
