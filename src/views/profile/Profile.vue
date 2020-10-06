<template>
  <div id="profile">
    <nav-bar class="nav-bar"><div slot="center">我的商城</div></nav-bar>
    <!--1.单独封装一个组件: 利用slot知识点-->
    <UserInfo ></UserInfo>

    <!--2.没有单独封装: 不同的地方太多, 需要传过多的参数-->
    <section class="account">
      <div class="account-item">
        <div class="number">
          <span class="balance">0.00</span>元
        </div>
        <div class="account-info">我的余额</div>
      </div>
      <div class="account-item">
        <div class="number">
          <span class="balance">0</span>个
        </div>
        <div class="account-info">我的优惠</div>
      </div>
      <div class="account-item">
        <div class="number">
          <span class="balance">0</span>分
        </div>
        <div class="account-info">我的积分</div>
      </div>
    </section>

    <!--3.封装成一个整体-->
    <list-view :list-data="orderList" class="order-list"></list-view>
    <list-view :list-data="serviceList" class="service-list"></list-view>
  </div>
</template>

<script>
  import UserInfo from './childComps/UserInfo'
  import ListView from './childComps/ListView'
  import NavBar from 'components/common/navbar/NavBar'

  export default {
    name: "Profile",
    components: {
      UserInfo, ListView, NavBar
    },
    data: function () {
      return {
        orderList: [
          {src: '~assets/img/profile/message.png' , info: '我的消息'},
          {src: '~assets/img/profile/shop.png' , info: '积分商城'},
          {src: '~assets/img/profile/vip.png' , info: '会员卡'},
        ],
        serviceList: [
          {src: '~assets/img/profile/cart.png' , info: '我的购物车'},
          {src: '~assets/img/profile/app.png' , info: '下载购物APP'},
        ]
      }
    },
    mounted: function () {
    }
  }
</script>

<style scoped>
  #profile {
    background-color: #f2f2f2;
  }

  .nav-bar {
    background-color: #ff8198;
    border-bottom: 1px solid grey;
    font-weight: 700;
    color: #fff;
  }

  .account {
    display: flex;
  }

  .account-item {
    width: 100%;
    background-color: #fff;
    /*margin-right: 1px;*/
    /*margin-left: 1px;*/
    /*text-align: center;*/
    flex: 1;
  }

  /*.account-item:last-of-type {*/
  /*  margin-right: 0;*/
  /*}*/

  .account-item {
    color: #666;
    font-size: 13px;
    padding: 18px;
  }

  .account-item .balance {
    font-size: 24px;
    font-weight: 700;
    color: #ff5f3e;
  }

  .number {
    text-align: center;
  }

  .account-info {
    margin-top: 6px;
    text-align: center;
  }

  .order-list, .service-list {
    margin-top: 12px;
  }

</style>
