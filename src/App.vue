<template>
  <div id="app">
    <!-- 调用日期选择器组件 -->
    <Datepicker 
      :format="format" 
      @change="changeDate"
    />
    <div class="time">{{time}}</div>
    <div class="time">基于vant-ui的移动端日期选择器</div>
    <p><button @click="handle(1)">YYYY-MM-DD</button></p>
    <p><button @click="handle(2)">YYYY/MM/DD</button></p>
    <p><button @click="handle(3)">YYYY年MM月DD日</button></p>
    <p><button @click="handle(4)">YYYYMMDD</button></p>
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
      time: '20190719',
      format: 'yyyy/mm/dd'
    }
  },
  methods: {
    // 改变选值
    changeDate(val){
      this.time = val
    },
    handle(type){
      const result = {
        1: 'YYYY-MM-DD',
        2: 'YYYY/MM/DD',
        3: 'YYYY年MM月DD日',
        4: 'YYYYMMDD'
      }
      this.format = result[type]
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
  line-height: 8rem;
  font-size: 1.5rem;
}
</style>
