<template>
  <div id="app">
    <keep-alive exclude="Detail">
      <router-view />
    </keep-alive>
    <MainTabBar></MainTabBar>
  </div>
</template>

<script>
  import MainTabBar from "components/content/mainTabbar/mainTabbar"
  export default {
    name: 'App',
    components: {
      MainTabBar
    }
  }
</script>

<style>
  #app {
    margin: -8px;
  }
</style>
