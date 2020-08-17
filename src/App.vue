<template>
  <div id="app">
    <router-view></router-view>
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
