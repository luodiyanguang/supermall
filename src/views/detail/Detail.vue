<template>
  <div class="detail">
    <detail-nav-bar class="detail-nav"/>
    <scroll class="content">
      <detail-swiper :top-images="topImages" />
      <detail-base-info :goods-info="goodsInfo" />
      <detail-shop-info :shop-info="shopInfo" />
      <detail-image-info :detail-info="detailInfo" />
      <detail-param-info :param-info="ParamInfo" />
      <detail-comment-info ref="comment" :comment-info="commentInfo"/>
      <good-list :goods="recommends"/>
    </scroll>
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



  import Scroll from "components/common/scroll/Scroll";
  import GoodList from "components/content/goods/GoodList"

  import {getDetail , Goods, getRecommend} from "network/detail";
  import {GoodsParam} from "../../network/detail";





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
      Scroll,
      GoodList
    },
    data() {
      return {
        iid: null,
        topImages: [],
        goodsInfo: {},
        shopInfo: {},
        detailInfo: {},
        ParamInfo: {},
        commentInfo: {},
        recommends: []
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
    height: calc(100% - 44px);
    overflow: hidden;
  }
</style>