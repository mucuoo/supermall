<template>
  <div>
    <nav-bar>
      <div slot="left" @click="backClick" class="back">
        <img src="~assets/img/common/back.svg" alt="">
      </div>
      <div slot="center" class="title">
        <div v-for="(item, index) in title" class="title-item"
             @click="itemClick(index)"
             :class="{active: currentIdx === index}">
          {{item}}
        </div>
      </div>
    </nav-bar>
  </div>
</template>

<script>
  import NavBar from "components/common/navbar/NavBar";

  export default {
    name: "DetailNavBar",
    components: {
      NavBar
    },
    data() {
      return {
        title: ['商品', '详情', '评论', '推荐'],
        currentIdx: 0
      }
    },
    methods: {
      itemClick(index) {
        this.currentIdx = index
      },
      backClick() {
        this.$router.back()
      }
    }
  }
</script>

<style scoped>
  .title {
    display: flex;
    padding: 0 20px;
    font-size: 14px;
  }

  .title-item {
    flex: 1;
  }

  .active {
    color: var(--color-high-text);
  }

  .back img {
    margin-top: 12px;
  }
</style>
