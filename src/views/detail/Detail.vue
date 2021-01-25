<template>
  <div id="detail">
    <detail-nav-bar class="detail-bar"/>
    <scroll class="wrapper" ref="scroll">
      <detail-swiper :top-img="topImg"/>
      <detail-base-info :goods="goods"/>
      <detail-shop-info :shop="shop"/>
      <detail-goods-info :detail-info="detailInfo" @imgLoad="imgLoad"/>
      <detail-param-info :param-info="paramInfo"/>
    </scroll>
  </div>
</template>

<script>
  import Scroll from "components/common/scroll/Scroll";
  import DetailNavBar from "./childComps/DetailNavBar";
  import DetailSwiper from "./childComps/DetailSwiper";
  import DetailBaseInfo from "./childComps/DetailBaseInfo";
  import DetailShopInfo from "./childComps/DetailShopInfo";
  import DetailGoodsInfo from "./childComps/DetailGoodsInfo";
  import DetailParamInfo from "./childComps/DetailParamInfo";

  import {getDetailData, Goods, Shop, GoodsParam} from "network/detail";

  export default {
    name: "Detail",
    components: {
      Scroll,
      DetailNavBar,
      DetailSwiper,
      DetailBaseInfo,
      DetailShopInfo,
      DetailGoodsInfo,
      DetailParamInfo
    },
    data() {
      return {
        iid: null,
        topImg: [],
        goods: {},
        shop: {},
        detailInfo: {},
        paramInfo: {}
      }
    },
    created() {
      this.iid = this.$route.params.id
      getDetailData(this.iid).then(res => {
        //1.获取数据
        console.log(res);
        const data = res.result

        //2.获取顶部的图片轮播图数据
        this.topImg = data.itemInfo.topImages

        //3.获取商品信息
        this.goods = new Goods(data.itemInfo, data.columns, data.shopInfo.services)

        //4.获取商家信息
        this.shop = new Shop(data.shopInfo)

        //5.获取详细信息
        this.detailInfo = data.detailInfo

        //6.保存参数信息
        this.paramInfo = new GoodsParam(data.itemParams.info, data.itemParams.rule)
      })
    },
    methods: {
      imgLoad() {
        this.$refs.scroll.refresh()
      }
    }
  }
</script>

<style scoped>
  #detail {
    position: relative;
    z-index: 9;
    background-color: #fff;
    height: 100vh;
  }

  .detail-bar {
    position: relative;
    z-index: 9;
    background-color: #fff;
  }

  .wrapper {
    height: calc(100% - 44px);
  }
</style>
