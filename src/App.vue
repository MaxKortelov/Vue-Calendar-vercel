<template>
  <div id="app">
    <Mainpage />
  </div>
</template>

<script>
import Mainpage from './components/mainpage/Mainpage.vue'

export default {
  name: 'App',
  components: {
    Mainpage
  }
}
</script>

<style>
  @import url(/fonts/Montserrat-Regular.ttf);

body {
  margin: 0;
  background-color: #F0F6FC;
  width: 1920px;
  height: 1080px;
  overflow: hidden;
  font-family: 'Montserrat', sans-serif;
  cursor: default;
}
</style>
