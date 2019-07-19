<template>
  <div id="app">
    <!-- 调用日期选择器组件 -->
    <Datepicker 
      :time="time" 
      :format="format" 
      @change="changeDate"
    />
    <div class="time">{{time}}</div>
    <div class="time">基于vant-ui的移动端日期选择器</div>
    <p><button @click="handle(1)">2020-02-20</button></p>
    <p><button @click="handle(2)">2020/02/20</button></p>
    <p><button @click="handle(3)">2020年02月20日</button></p>
    <p><button @click="handle(4)">20200220</button></p>
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
      format: 'yyyy年mm月dd日'
    }
  },
  methods: {
    // 接收选值
    changeDate(val){
      this.time = val
    },
    handle(type){
      let result = {
        1: '2020-02-20',
        2: '2020/02/20',
        3: '2020年02月20日',
        4:'20200220'
      }
      this.time = result[type]
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
