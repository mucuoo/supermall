<template>
  <div class="tab-control">
    <div v-for="(item,index) in title"
         @click="itemClick(index)"
         class="tab-control-item"
         :class="{active: currentIdx === index}">
      <span>{{item}}</span>
    </div>
  </div>
</template>

<script>
  export default {
    name: "TabControl",
    data() {
      return {
        currentIdx: 0
      }
    },
    props: {
      title: {
        type: Array,
        default() {
          return []
        }
      }
    },
    methods: {
      itemClick(index) {
        this.currentIdx = index
        this.$emit('tabClickInner', index)
        this.$emit('tabClickOuter', index)
      }
    }
  }
</script>

<style scoped>
  .tab-control {
    display: flex;
    text-align: center;
    font-size: 15px;
    height: 40px;
    line-height: 40px;
    background-color: #fff;
  }

  .tab-control-item {
    flex: 1;
  }

  .tab-control-item span {
    padding: 5px;
  }

  .active {
    color: var(--color-high-text);
  }

  .active span {
    border-bottom: 3px solid var(--color-tint);
  }
</style>
