<template>
  <div id="app">
    <div class="head">
      <img src="./assets/logo.png">
      <h1>Playvue</h1>
    </div>
    <playvue :file="wings"></playvue>
  </div>
</template>

<script>
import wings from './assets/Wings.mp3'
import Playvue from './components/Playvue.vue'

export default {
  name: 'app',
  components: { Playvue },
  data () {
    return {
      wings
    }
  }
}
</script>

<style>
* {
  box-sizing: border-box;
}

body {
  overflow-x: hidden;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}

h1 {
  font-weight: normal;
}

.head {
  text-align: center;
}
</style>
