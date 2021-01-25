<template>
  <div id="home">
    <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>
    <tab-control :title="['流行','新款','精选']"
                 @tabClickOuter="tabClickOuter"
                 ref="tabControlOuter"
                 class="fix"
                 v-show="isTabFixed"/>
    <scroll class="home-wrapper"
            ref="wrapper"
            :probe-type="3"
            @scroll="scrollPosition"
            :pull-up="true"
            @pullUp="moreContent">
      <home-swiper :banner="banner" @swiperImgLoad="swiperImgLoad"></home-swiper>
      <recommend-view :recommend="recommend"></recommend-view>
      <feature-view/>
      <tab-control :title="['流行','新款','精选']" @tabClickInner="tabClickInner" ref="tabControlInner"/>
      <goods-list :goods="showGoods"/>
    </scroll>
    <back-top @click.native="backClick" v-show="isShowBack"/>
  </div>
</template>

<script>
  import HomeSwiper from "./childComps/HomeSwiper";
  import RecommendView from "./childComps/RecommendView";
  import FeatureView from "./childComps/FeatureView";

  import NavBar from "components/common/navbar/NavBar";
  import TabControl from "components/content/tabControl/TabControl";
  import GoodsList from "components/content/goods/GoodsList";
  import Scroll from "components/common/scroll/Scroll";
  import BackTop from "components/content/backTop/BackTop";

  import {getHomeMultiData, getHomeGoods} from "network/home";
  import {debounce} from "common/utils";

  export default {
    name: "Home",
    data() {
      return {
        banner: [],
        recommend: [],
        goods: {
          'pop': {page: 0, list: []},
          'new': {page: 0, list: []},
          'sell': {page: 0, list: []}
        },
        currentType: 'pop',
        isShowBack: false,
        tabOffsetTop: 0,
        isTabFixed: false,
        currentY: 0
      }
    },
    computed: {
      showGoods() {
        return this.goods[this.currentType].list
      }
    },
    components: {
      HomeSwiper,
      RecommendView,
      FeatureView,
      NavBar,
      TabControl,
      GoodsList,
      Scroll,
      BackTop
    },
    created() {
      //请求多个数据
      this.getHomeMultiData()

      //请求商品数据
      this.getHomeGoods('pop')
      this.getHomeGoods('new')
      this.getHomeGoods('sell')
    },
    mounted() {
      //图片加载完成的事件监听
      const refresh = debounce(this.$refs.wrapper.refresh, 50)
      this.$bus.$on('imgLoad', () => {
        refresh()
      })
    },
    activated() {
      this.$refs.wrapper.refresh()
      this.$refs.wrapper.scrollTo(0, this.currentY, 0)
    },
    deactivated() {
      this.currentY = this.$refs.wrapper.getCurrentY()
    },
    methods: {
      /**
       * 事件监听相关方法
       */
      tabClickInner(index) {
        if(index === 0){
          this.currentType = 'pop'
        }else if(index === 1){
          this.currentType = 'new'
        }else{
          this.currentType = 'sell'
        }
        this.$refs.tabControlOuter.currentIdx = index
      },
      tabClickOuter(index) {
        if(index === 0){
          this.currentType = 'pop'
          this.$refs.wrapper.scrollTo(0, -this.tabOffsetTop, 250)
        }else if(index === 1){
          this.currentType = 'new'
          this.$refs.wrapper.scrollTo(0, -this.tabOffsetTop, 250)
        }else{
          this.currentType = 'sell'
          this.$refs.wrapper.scrollTo(0, -this.tabOffsetTop, 250)
        }
        this.$refs.tabControlInner.currentIdx = index
      },
      backClick() {
        this.$refs.wrapper.scrollTo(0, 0)
      },
      scrollPosition(position) {
        //判断backTop是否显示
        this.isShowBack = (-position.y) > 1000

        //判断tabControl是否吸顶
        this.isTabFixed = (-position.y) > this.tabOffsetTop
      },
      moreContent() {
        this.getHomeGoods(this.currentType)
        this.$refs.wrapper.finishPullUp()
      },
      swiperImgLoad() {
        this.tabOffsetTop = this.$refs.tabControlInner.$el.offsetTop
      },
      /**
       * 网络请求相关方法
       */
      getHomeMultiData() {
        getHomeMultiData().then(res => {
          this.banner = res.data.banner.list
          this.recommend = res.data.recommend.list
        })
      },
      getHomeGoods(type) {
        const page = this.goods[type].page + 1
        getHomeGoods(type, page).then(res => {
          this.goods[type].list.push(...res.data.list)
          this.goods[type].page += 1
        })
      }
    }
  }
</script>

<style scoped>
  #home {
    height: 100vh;
    position: relative;
  }

  .home-nav {
    background-color: var(--color-tint);
    color: #fff;

    /*用于网页原生滚动的样式*/
    /*position: fixed;*/
    /*left: 0;*/
    /*right: 0;*/
    /*top: 0;*/
    /*z-index: 9;*/
  }

  .home-wrapper {
    position: fixed;
    top: 44px;
    bottom: 49px;
    left: 0;
    right: 0;
    overflow: hidden;
  }

  .fix {
    position: relative;
    z-index: 9;
  }
</style>
