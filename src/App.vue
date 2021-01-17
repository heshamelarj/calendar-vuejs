<template>
  <div id="app">
    <Calendar msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import Calendar from './components/calendar/Calendar.vue'

export default {
  name: 'App',
  components: {
    Calendar
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
  display: flex;
  align-items: center;
  align-content: center;
  width: 100%;
}
</style>
