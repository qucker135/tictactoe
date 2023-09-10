<template>
  <img alt="Vue logo" src="./assets/cross.png" class="header-pics">
  <img alt="Vue logo" src="./assets/nough.png" class="header-pics">
  <h1>Tic-tac-toe</h1>
  <GamePanel />
</template>

<script>
import GamePanel from './components/GamePanel.vue'

export default {
  name: 'App',
  components: {
    GamePanel
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #fff;
  margin-top: 60px;
}
.header-pics {
  width: 100px;
  height: auto;
  margin-left:10px;
  margin-right: 10px;
}
</style>
