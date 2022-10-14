<template>
  <div id="app">
    <ImageToText></ImageToText>
  </div>
</template>

<script>
import ImageToText from './components/ImageToText.vue';

export default {
  name: 'App',
  components: { ImageToText }
}
</script>

<style>
html, body {
  padding: 0;
  margin: 0;
  height: 100%;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  height: 100%;
}
</style>
