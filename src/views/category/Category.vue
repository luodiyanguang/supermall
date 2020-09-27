<template>
  <div id="category">
    <nav-bar class="category_nav"><template #center>分类</template></nav-bar>
    <div class="main">
      <category-left ref="left"></category-left>
      <category-right ref="right" :list_item="rightList"></category-right>
    </div>
  </div>
</template>
<script>
  import { getCategoryInfo } from "network/category.js";

  import CategoryLeft from "./childComps/CategoryLeft.vue";
  import CategoryRight from "./childComps/CategoryRight.vue";

  import NavBar from "components/common/navbar/NavBar.vue";
  export default {
    data() {
      return {
        rightList: null
      };
    },
    created() {},
    mounted() {
      // console.log(this.$refs.left);
      // console.log(this.$refs.left.$el.style);
    },
    components: {
      NavBar,
      CategoryLeft,
      CategoryRight
    },
    methods: {

      getCategoryInfo(maitKey) {
        getCategoryInfo(maitKey).then(res => {
          this.rightList = res.data.list
          // console.log(this.rightList = res.data)
          // console.log(this.rightList)
        })
      }
    }
  };
</script>
<style scoped>
  .category_nav {
    background-color: #ff8198;
    color: white;
  }
  .main {
    display: flex;
    font-size: 0.6rem;
    height: calc(100vh - 2.09rem - 1.9rem);
  }
</style>
