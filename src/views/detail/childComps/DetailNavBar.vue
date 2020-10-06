<template>
  <div class="detailNavbar">
    <nav-bar>
      <div slot="left" class="goback" @click="gobackClick">
        <img src="~assets/img/common/left.png" />
      </div>
      <div slot="center" class="title">
        <div v-for="(item, index) in titles"
             class="item-title"
             :class="{active: index === currentIndex}"
             @click="titleClick(index)">{{item}}</div>
      </div>
    </nav-bar>
  </div>
</template>

<script>
  import NavBar from 'components/common/navbar/NavBar'

  export default {
    name: "DetailNavBar",
    components: {
      NavBar
    },
    data() {
      return {
        titles: ['商品','参数','评价','推荐'],
        currentIndex: 0
      }
    },
    methods: {
      gobackClick() {
        this.$router.back(-1)
      },
      titleClick(index) {
        this.currentIndex = index
        this.$emit('titleClick', index)
      }
    }
  }
</script>

<style scoped>
  .goback img{
    width: 20px;
    vertical-align: middle;
  }

  .title {
    display: flex;
  }
  .item-title {
    flex: 1;
    font-size: 13px;
  }
  .active {
    color: #ff5777;
  }
</style>