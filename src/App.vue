<template>
  <div id="app">
    <p>
      <img src="./assets/logo.png">
      <h1>Playvue</h1>
    </p>
    
    <playvue :file="turbo"></playvue>

    <p>
      <em>Twilight Turbo Drive by <a href="https://dangermode.bandcamp.com/">Danger Mode</a></em>
    </p>
  </div>
</template>

<script>
import turbo from './assets/Twilight_Turbo_Drive.mp3'
import Playvue from './components/Playvue.vue'

export default {
  name: 'app',
  components: { Playvue },
  data () {
    return {
      turbo
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
  text-align: center;
}

h1 {
  font-weight: normal;
}

.head {
}
</style>
