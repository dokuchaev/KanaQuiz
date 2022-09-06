<template>
  <KanaQuiz />
</template>
<style>
  @import './assets/styles/style.css';
</style>
<script>
import KanaQuiz from './components/KanaQuiz.vue'

export default {
  name: 'App',
  components: {
    KanaQuiz
  }
}

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
