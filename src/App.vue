<template>
  <div id="app">
    <router-view />
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style lang="scss" scoped>

  html, body {
    width: 100%;
    overflow-y:scroll;
    height: 300px;
    font-family: 'Microsoft Yahei' !important;
  }
  #app,.wrapper{
    width:100%;
    position:absolute;
    overflow-y: scroll;
  }
  @media screen and (min-width: 769px) {
    body{
      box-shadow: 0 0 30px gray;
      margin:0 auto;
    }
    body,#app{
      width:840px
    }
  }
</style>
