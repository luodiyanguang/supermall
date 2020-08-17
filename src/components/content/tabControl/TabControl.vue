<template>
  <div class="tab-control">
    <div v-for="(item , index) in titles"
         class="tab-control-item"
         :class="{active: index === currentIndex}" @click="itemclick(index)">
      <span>{{item}}</span></div>
  </div>
</template>

<script>
  export default {
    name: "TabControl",
    data() {
      return {
        currentIndex: 0
      }
    },
    props: {
      titles: {
        type: Array,
        default() {
          return []
        }
      }
    },
    methods: {
      itemclick(index) {
        this.currentIndex = index
        this.$emit('tabClick', index)
      }
    }
  }
</script>

<style scoped>
  .tab-control {
    display: flex;
    font-size: 15px;
    text-align: center;
    height: 40px;
    line-height: 40px;
    background: #fff;
  }
  .tab-control-item {
    flex: 1;
  }
  .active span {
    color: #ff5777;
    border-bottom: 3px solid #ff8198;
  }
</style>