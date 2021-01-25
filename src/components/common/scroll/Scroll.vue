<template>
  <div ref="wrapper">
    <div>
      <slot></slot>
    </div>
  </div>
</template>

<script>
  import BScroll from '@better-scroll/core'
  import ObserveDOM from '@better-scroll/observe-dom'
  import PullUp from "@better-scroll/pull-up";

  BScroll.use(ObserveDOM)
  BScroll.use(PullUp)

  export default {
    name: "Scroll",
    data() {
      return {
        scroll: null
      }
    },
    props: {
      probeType: {
        type: Number,
        default: 0
      },
      pullUp: {
        type: Boolean,
        default: false
      }
    },
    mounted() {
      this.scroll = new BScroll(this.$refs.wrapper, {
        click: true,
        disableMouse: true,
        disableTouch: false,
        observeDOM: true,
        probeType: this.probeType,
        pullUpLoad: this.pullUp
      })

      if(this.probeType === 2 || this.probeType === 3){
        this.scroll.on('scroll', (position) => {
          this.$emit('scroll', position)
        })
      }

      if (this.pullUp){
        this.scroll.on('pullingUp', () => {
          this.$emit('pullUp')
        })
      }
    },
    methods: {
      scrollTo(x, y, time=500) {
        this.scroll && this.scroll.scrollTo(x, y, time)
      },
      finishPullUp() {
        this.scroll && this.scroll.finishPullUp()
      },
      refresh() {
        this.scroll && this.scroll.refresh()
      },
      getCurrentY() {
        return this.scroll ? this.scroll.y : 0
      }
    }
  }
</script>

<style scoped>

</style>
