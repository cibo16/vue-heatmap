<template>
  <Calender :dates="dates"/>
</template>

<script>
import Calender from './components/Calender.vue'
import data from './assets/data.json'

export default {
  name: 'App',
  data () {
    return {
      dates: null,
    }
  },
  created () {
    this.dates = data;
  },
  components: {
    Calender
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
