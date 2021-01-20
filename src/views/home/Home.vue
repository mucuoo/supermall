<template>
  <div id="home">
    <nav-bar><div slot="center">购物街</div></nav-bar>
    <home-swiper :banner="banner"></home-swiper>
    <recommend-view :recommend="recommend"></recommend-view>
  </div>
</template>

<script>
  import NavBar from "components/common/navbar/NavBar";
  import HomeSwiper from "./childComps/HomeSwiper";
  import RecommendView from "./childComps/RecommendView";

  import {getHomeMultiData} from "network/home";

  export default {
    name: "Home",
    data() {
      return {
        banner: [],
        recommend: []
      }
    },
    components: {
      NavBar,
      HomeSwiper,
      RecommendView
    },
    created() {
      getHomeMultiData().then(res => {
        this.banner = res.data.banner.list
        this.recommend = res.data.recommend.list
      }, err => {
        console.log(err);
      })
    }
  }
</script>

<style scoped>
  #home {
    background-color: var(--color-tint);
    color: #f6f6f6;
  }
</style>
