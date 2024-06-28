<template>
  <div id="app">
    <circle-counter :value="value" text="label"/>
    <input type="range" v-model.number="value"  min="0" max="108">
  </div>
</template>

<script>
import CircleCounter from '../src/components/ColorPalette.vue'

export default {
  name: 'app',
  components: {
    CircleCounter
  },
  data () {
    return {
      value: 0,
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 0 auto;
  width: 100%;
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;

  input {
    max-width: 180px;
  }
}

body {
  background: #2c3e50;
}
</style>
