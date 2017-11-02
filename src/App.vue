<template>
  <div id="app">
    <div class="parent_data"> {{ start }} - {{ end }} </div>
    <Calendar 
      @sendDate="getDate" 
      :setDays="setDays" 
      :placeholderTextStartDay="textStartDay" 
      :placeholderTextEndDay="textEndDay" />
  </div>
</template>

<script>
import Calendar from './components/Calendar'

export default {
  name: 'app',
  components: {
    Calendar
  },
  data() {
    return {
      setDays: ['2017/11/2', '2018/11/2'],
      start: '',
      end: '',
      textStartDay: 'placeholderTextStartDay',
      textEndDay: 'placeholderTextEndDay',
    }
  },
  mounted() {
    this.setPropValue();
  },
  computed: {},
  methods: {
    setPropValue() {
      this.start = this.setDays[0];
      this.end = this.setDays[1];
    },
    getDate({ startDay, endDay } = {}) {
      this.start = startDay;
      this.end = endDay;
    },
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
  margin-top: 60px;
}
.parent_data {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  margin-bottom: 10px;
}
</style>
