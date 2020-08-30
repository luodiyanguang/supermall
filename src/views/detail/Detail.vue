<template>
  <div class="detail">
    <detail-nav-bar class="detail-nav" @titleClick="titleClick" ref="nav" />
    <scroll class="content" ref="scroll"
            :probeType="3" @scroll="contentScroll">

      <ul>
        <li v-for="item in $store.state.cartList">{{item}}</li>
      </ul>
      <detail-swiper :top-images="topImages" />
      <detail-base-info :goods-info="goodsInfo" />
      <detail-shop-info :shop-info="shopInfo" />
      <detail-image-info :detail-info="detailInfo" @detailImageLoad="detailImageLoad"/>
      <detail-param-info :param-info="ParamInfo" ref="params"/>
      <detail-comment-info ref="comment" :comment-info="commentInfo"/>
      <good-list :goods="recommends" ref="recommend"/>
    </scroll>
    <back-top v-show="isShow" @click.native="backTop" />
    <detail-button-bar class="bottom-bar" @addToCart="addToCart"/>
  </div>
</template>

<script>
  import DetailNavBar from "./childComps/DetailNavBar";
  import DetailSwiper from "./childComps/DetailSwiper";
  import DetailBaseInfo from "./childComps/DetailBaseInfo";
  import DetailShopInfo from "./childComps/DetailShopInfo";
  import DetailImageInfo from "./childComps/DetailImageInfo";
  import DetailParamInfo from "./childComps/DetailParamInfo";
  import DetailCommentInfo from "./childComps/DetailCommentInfo";
  import DetailButtonBar from "./childComps/DetailButtonBar";



  import Scroll from "components/common/scroll/Scroll";
  import GoodList from "components/content/goods/GoodList"

  import {getDetail , Goods, getRecommend} from "network/detail";
  import {GoodsParam} from "network/detail";
  import {itemListerMixin, backTopMixin} from "common/mixin";
  import {debounce} from "common/utils";
  // import BackTop from "components/content/backTop/BackTop";






  export default {
    name: "Detail",
    components: {

      DetailSwiper,
      DetailNavBar,
      DetailBaseInfo,
      DetailShopInfo,
      DetailImageInfo,
      DetailParamInfo,
      DetailCommentInfo,
      DetailButtonBar,
      Scroll,
      GoodList
    },
    mixins: [itemListerMixin, backTopMixin],
    data() {
      return {
        iid: null,
        topImages: [],
        goodsInfo: {},
        shopInfo: {},
        detailInfo: {},
        ParamInfo: {},
        commentInfo: {},
        recommends: [],
        themeTopYs: [0, 0 , 0, 0],
        getThemeTopY: null,
        currentIndex: 0
      }
    },
    created() {
      //从首页跳转时传入被点击商品的iid
      this.iid = this.$route.params.iid

     //通过iid获得商品信息
      getDetail(this.iid).then(res => {
        //获取顶部轮播图信息
        console.log(res)
        const data = res.result;
        this.topImages = data.itemInfo.topImages

        //获取商品信息
        this.goodsInfo = new Goods(data.itemInfo, data.columns, data.shopInfo.services)
        console.log(this.goodsInfo)

        //获取店铺信息
        this.shopInfo = data.shopInfo

        //获取商品图片信息
        this.detailInfo = data.detailInfo

        //获取商品参数信息
        this.ParamInfo = new GoodsParam(data.itemParams.info, data.itemParams.rule)

        // 7.保存评论数据
        if (data.rate.list) {
          this.commentInfo = data.rate.list[0];
        }
      })

      //请求推荐数据
      getRecommend().then(res => {
        this.recommends = res.data.list
      })

      //给getThemeTopY赋值
      this.getThemeTopY = debounce(() => {
        this.themeTopYs = []
        this.themeTopYs.push(0)
        this.themeTopYs.push(this.$refs.params.$el.offsetTop)
        this.themeTopYs.push(this.$refs.comment.$el.offsetTop)
        this.themeTopYs.push(this.$refs.recommend.$el.offsetTop)
        this.themeTopYs.push(Number.MAX_VALUE)
        console.log(this.themeTopYs)
      }, 100)
    },
    mounted() {

    },
    destroyed() {
      this.$bus.$off('itemImgLoad' , this.itemImgListener)
    },
    methods: {
      detailImageLoad() {
        // 0
        this.newRefresh()
        this.getThemeTopY()
      },
      titleClick(index) {
        this.$refs.scroll.scrollTo(0, -this.themeTopYs[index] , 100)
      },
      // contentScroll(position) {
      //   const positionY = -position.y
      //   let length = this.themeTopYs.length
      //   for( let i = 0; i < length; i++){
      //     if(this.currentIndex !== i && ((i < length - 1 && positionY
      //     >= this.themeTopYs[i] && positionY < this.themeTopYs[i + 1]) ||
      //         (i === length -1 && positionY >= this.themeTopYs[i]))){
      //       this.currentIndex = i
      //       // console.log(this.currentIndex)
      //       this.$refs.nav.currentIndex = this.currentIndex
      //     }
      //   }
      // }
      contentScroll(position) {
        const positionY = - position.y
        let length = this.themeTopYs.length
        for(let i = 0; i < length-1; i++) {
          if((this.currentIndex !== i) && (positionY >= this.themeTopYs[i] && positionY < this.themeTopYs[i+1])){
            this.currentIndex = i
            this.$refs.nav.currentIndex = this.currentIndex
          }
        }

        //是否要显示返回顶部的按钮
        this.listenShowBackTop(position)
      },
      addToCart() {
        //将商品信息添加到Store中
        const obj = {}
        obj.iid = this.iid
        obj.imgURL = this.topImages[0]
        obj.title = this.goodsInfo.title
        obj.desc = this.goodsInfo.desc
        obj.price = this.goodsInfo.realPrice

        //将商品添加到购物车
        // this.$store.commit('addCart', obj )
        this.$store.dispatch('addCart', obj)
      }
    }
  }
</script>
<style scoped>
  .detail {
    position: relative;
    background: #fff;
    height: 100vh;
    z-index: 9;
  }

  .detail-nav{
    position: relative;
    background-color: #fff;
    z-index: 9;
  }

  .content  {
    height: calc(100% - 44px - 49px);
    overflow: hidden;
  }
</style>