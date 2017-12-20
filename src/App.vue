<template>
  <div id="app">
    <div class="calendar">
      <h1>Calendar is noraml</h1>
      <div class="parent_data"> Limit Days: {{ limitStart }} - {{ limitEnd }} </div>
      <div class="parent_data"> Current Days: {{ currentStart }} - {{ currentEnd }} </div>
      <Calendar 
        @sendDate="getDate" 
        :datepickerStatus="datepickerStatusEmpty"
        :setLimitDays="setLimitDays"
        :setCurrentDays="setCurrentDays" />

      <h1>Calendar is disable</h1>
      <Calendar 
        @sendDate="getDate" 
        :datepickerStatus="datepickerStatus"
        :setLimitDays="setLimitDays"
        :setCurrentDays="setCurrentDays" />
    </div>
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
      setLimitDays: ['', ''],
      limitStart: '',
      limitEnd: '',

      setCurrentDays: ['2015/11/2', '2018/11/2'],
      currentStart: '',
      currentEnd: '',
      
      datepickerStatus: 'disable',
      datepickerStatusEmpty: '',
    }
  },
  mounted() {
    this.setPropValue();
  },
  computed: {},
  methods: {
    setPropValue() {
      this.limitStart = this.setLimitDays[0];
      this.limitEnd = this.setLimitDays[1];
      this.currentStart = this.setCurrentDays[0];
      this.currentEnd = this.setCurrentDays[1];
    },
    getDate({ startDay, endDay } = {}) {
      this.currentStart = startDay;
      this.currentEnd = endDay;
    },
  }
}
</script>

<style>

html, body {
  width: 100%;
  height: 100%;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#app .calendar {
  display: flex;
  flex-direction: column;
}

#app .calendar .calendar_wrapper {
  margin: 20px auto;
}

.parent_data {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  margin-bottom: 10px;
}
</style>
