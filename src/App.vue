<template>
  <div id="app">
    <Datepicker :time="time" @change="changeDate"/>
    <div class="time">{{time}}</div>
  </div>
</template>

<script>
import Datepicker from './components/Datepicker.vue'

export default {
  name: 'app',
  components: {
    Datepicker
  },
  data() {
    return {
      time: '2019-07-17'
    }
  },
  methods: {
    changeDate(val){
      this.time = val
    }
  },
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.time{
  line-height: 10rem;
  color: red;
}
</style>
