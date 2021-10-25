<template>
  <div id="app" class="w-100 h-100">
    <Index />
  </div>
</template>

<script>
import Index from './pages/Index.vue';

export default {
  name: 'App',
  components: {
    Index
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap');
#app {
  font-family: 'Press Start 2P', cursive;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
body{
  background:#0288D1;
  color:white;
}
</style>
