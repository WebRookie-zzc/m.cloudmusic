<template>
  <view>
    <SearchBar/>
    <Banner/>
    <RecommendBar/>
  </view>
</template>

<script>
  import SearchBar from '../../component/Find/SearchBar'
  import Banner from '../../component/Find/Banner'
  import RecommendBar from '../../component/Find/RecommendBar'

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
      RecommendBar
    },

    onLoad() {

    }
  }
</script>

<style>

</style>
