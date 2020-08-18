<template>
  <div id="home">
    <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>
    <scroll class="content" ref="scroll"
            :probe-type="3" :pull-up-load="true"
            @scroll="contentScroll" @pullingUp="loadMore">
      <home-swiper :banners="banners" />
      <recommend-view :recommends="recommends" />
      <feature-view />
      <tab-control class="tab-control"
                   :titles="['流行','新型','精选']" @tabClick="tabClick"/>
      <good-list :goods="showGoods"/>
    </scroll>
    <back-top @click.native="backClick" v-show="isShow"/>
  </div>
</template>

<script>
  import NavBar from "components/common/navbar/NavBar";
  import HomeSwiper from "./childComps/HomeSwiper";
  import RecommendView from "./childComps/RecommendView";
  import FeatureView from "./childComps/FeatureView";
  import GoodList from "../../components/content/goods/GoodList";


  import TabControl from "components/content/tabControl/TabControl";
  import Scroll from "components/common/scroll/Scroll";
  import BackTop from "components/content/backTop/BackTop";

  import {getHomeMultidata , getHomeGoods } from 'network/home'


  export default {
    name: "Home",
    components: {

      NavBar,
      HomeSwiper,
      RecommendView,
      FeatureView,
      TabControl,
      GoodList,
      Scroll,
      BackTop
    },
    data() {
      return {
        // result: null
        banners: [],
        recommends: [],
        goods: {
          'pop': {page: 0, list: []},
          'new': {page: 0, list: []},
          'sell': {page: 0, list: []}
        },
        currentType: 'pop',
        isShow: false
      }
    },
    created() {
      this.getHomeMultidata()

      //请求商品数据
      this.getHomeGoods('pop')
      this.getHomeGoods('new')
      this.getHomeGoods('sell')


    },
    mounted() {
      //监听item中图片加载完成
      // this.$bus.$on('itemImageLoad', () => {
      //   this.$refs.scroll && this.$refs.scroll.refresh()
      //   console.log('---')
      // })
      const refresh = this.debounce(this.$refs.scroll.refresh , 200)
      this.$bus.$on('itemImageLoad' , () => {
        refresh()
      })
    },
    methods: {
      /*
      * 事件监听
      *
      * */
      debounce(func , delay) {
        let timer = null
        return function (...args) {
          if (timer) clearTimeout(timer)
          timer = setTimeout(() => {
            func.apply(this, args)
          }, delay)
        }
      },

      tabClick(index) {
        switch(index) {
          case 0:
            this.currentType = 'pop'
            break
          case 1:
            this.currentType = 'new'
            break
          case 2:
            this.currentType = 'sell'
            break
        }
      },
      //返回顶部
      backClick() {
        return this.$refs.scroll.scrollTo(0 , 0)
      },
      //小图标的消失与存在
      contentScroll(position) {
        this.isShow = -(position.y) > 1000
      },
      //上拉加载更多
      loadMore() {
        this.getHomeGoods(this.currentType)
        // console.log('上拉加载更多')
      },


      /*
       *
       *  网络请求相关
      */
      getHomeMultidata() {
        getHomeMultidata().then(res => {
          // this.result = res
          this.banners = res.data.banner.list;
          this.recommends = res.data.recommend.list;
        })
      },
      getHomeGoods(type) {
        const page = this.goods[type].page + 1
        getHomeGoods(type , page).then(res => {
          this.goods[type].list.push(...res.data.list)
          this.goods[type].page += 1

          this.$refs.scroll.finishPullUp()
        })
      }
    },
    computed: {
      showGoods() {
        return this.goods[this.currentType].list
      }
    }
  }
</script>

<style scoped>
  #home {
    /*padding-top: 44px;*/
    position: relative;
    height: 100vh;

  }
  .home-nav {
    background-color: #ff8198;
    /*background-color: var(--color-tint);*/
    color: #666;

    position: fixed;
    left: 0;
    right: 0;
    top: 0;
    z-index: 9;
  }
  .tab-control {
    position: sticky;
    top:  44px;
    z-index: 9;
  }

  .content {
    overflow: hidden;

    position: absolute;
    top: 44px;
    bottom: 49px;
    left: 0;
    right: 0;
  }
</style>
