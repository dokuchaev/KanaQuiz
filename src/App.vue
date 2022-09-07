<template>
  <ThemeButton />
  <KanaQuiz />
</template>
<style>
  @import './assets/styles/style.css';
  @import './assets/styles/state.css';

</style>
<script>
import KanaQuiz from './components/KanaQuiz.vue'
import ThemeButton from './components/ThemeButton.vue'
export default {
  name: 'App',
  components: {
    KanaQuiz,
    ThemeButton
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
