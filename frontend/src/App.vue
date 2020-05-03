<template>
  <div id="app">
    <h2>Uplaod Your File Here</h2>
    <FileUpload />
  </div>
</template>

<script>
import FileUpload from './components/FileUpload.vue'

export default {
  name: 'App',
  components: {
    FileUpload
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
