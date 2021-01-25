<template>
  <swiper class="detail-swiper">
    <swiperItem v-for="item in topImg" :key="item">
      <img :src="item" alt="">
    </swiperItem>
  </swiper>
</template>

<script>
  import {Swiper, SwiperItem} from 'components/common/swiper'

  export default {
    name: "DetailSwiper",
    props: {
      topImg: {
        type: Array,
        default: function() {
          return []
        }
      }
    },
    components: {
      Swiper,
      SwiperItem
    }
  }
</script>

<style scoped>
  .detail-swiper {
    height: 350px;
    overflow: hidden;
  }
</style>
