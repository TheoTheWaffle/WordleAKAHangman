<template>
  <div id="app">
    <Game />
  </div>
</template>

<script>
import Game from './components/Game.vue';

export default {
  name: 'App',
  components: {
    Game
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
