<template>
  <div id="app">
    <Header></Header>
    <router-view class="router" :key="$route.fullPath"></router-view>
  </div>
</template>

<script>
  import Header from './components/Header'
  export default {
    name: 'App',
    components: {Header}
  }
</script>

<style scoped>
  .router{
    margin-top: 100px;
    flex: 5;
  }
</style>
