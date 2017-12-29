<template>
  <div for="" class="calendar_wrapper" :class="datepickerStatus">
    <template v-if="datepickerStatus === 'disable'">
      <label>
        <input v-model="startDay" type="text" readonly="readonly">
          <svg class="enabled" type="clear" viewBox="0 0 20 20"><use xlink:href="#clear"></use></svg>
          <svg type="select" viewBox="0 0 11 6"><use xlink:href="#select"></use></svg>
      </label>
      <span class="divider"> ~ </span>
      <label>
        <input v-model="endDay" type="text" readonly="readonly">
          <svg class="enabled" type="clear" viewBox="0 0 20 20"><use xlink:href="#clear"></use></svg>
          <svg type="select" viewBox="0 0 11 6"><use xlink:href="#select"></use></svg>
      </label>
    </template>
    <template v-else>
      <label>
        <input 
          :placeholder="placeholderTextStartDay"
          :class="{ active: activeTypeDay === 'start' }"
          v-model="startDay"
          type="text" 
          readonly="readonly"
          @click="toggleCalendar('start')">
          <svg v-if="startDay" :class="{ enabled: startDay }" type="clear" viewBox="0 0 20 20" @click="clearDateValue('start')"><use xlink:href="#clear"></use></svg>
          <svg type="select" viewBox="0 0 11 6"><use xlink:href="#select"></use></svg>
      </label>
      <span class="divider"> ~ </span>
      <label>
        <input 
          :placeholder="placeholderTextEndDay"
          :class="{ active: activeTypeDay === 'end' }"
          v-model="endDay"
          type="text" 
          readonly="readonly"
          @click="toggleCalendar('end')">
          <svg v-if="endDay" :class="{ enabled: endDay }" type="clear" viewBox="0 0 20 20" @click="clearDateValue('end')"><use xlink:href="#clear"></use></svg>
          <svg type="select" viewBox="0 0 11 6"><use xlink:href="#select"></use></svg>
      </label>
    </template>

    <div class="calendar_content" :class="{ open: calendarStatus }">
      <div class="calendar-action">
        <div class="action-move" @click="prevYear(-1)">
          <div title="prevYear"><svg type="double_next" viewBox="0 0 284.936 284.936"><use xlink:href="#double_arrow"></use></svg></div>
        </div>
        <div class="action-move" @click="prevMonth(-1)">
          <div title="prevMonth"><svg type="prev" viewBox="0 0 20 12"><use xlink:href="#arrow"></use></svg></div>
        </div>
        <div class="action-text">{{ calendarTitle }}</div>
        <div class="action-move" @click="nextMonth(1)">
          <div title="nextMonth"><svg type="next" viewBox="0 0 20 12"><use xlink:href="#arrow"></use></svg></div>
        </div>
        <div class="action-move" @click="nextYear(1)">
          <div title="nextYear"><svg type="double_prev" viewBox="0 0 284.936 284.936"><use xlink:href="#double_arrow"></use></svg></div>
        </div>
      </div>
      <div class="calendar-now-time">
        <div class="prev_month">{{ month === 0 ? 12 : month }}月</div>
        <div class="year">{{ year }}年 / {{ month + 1 }}月</div>
        <div class="next_month">{{ month + 2 === 13 ? 1 : month + 2 }}月</div>
      </div>
      <div class="week_wrapper">
        <div class="week_list">
          <div class="" v-for="(list, z) in weekList" :key="z">{{ list }}</div>
        </div>
        <div class="week" v-for="(w, i) in weeks" :key="i">
          <div 
            @click="clickDate(d)" 
            v-for="(d, j) in w" 
            :key="j" 
            :class="{
              current_selected_range: currentSelectdRange(d) === true,
              default_start_day: setDaultStartDayClass(d),
              default_end_day: setDaultEndDayClass(d),
              out_default_range: d.default_range === false,
              current_day: d.current === true,
            }"
            :data-year="d.year"
            :data-month="d.month"
            :data-date="d.date">{{ d.date }}</div>
        </div>
      </div>

      <svg style="display: none;">
        <symbol>
          <g id="select" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd">
            <g id="Selectbox/DatePicker" transform="translate(-136.000000, -18.000000)" fill="#000000">
              <path d="M147,18.6666667 C147,18.8472231 146.931967,19.0034716 146.795898,19.1354167 L141.983398,23.8020833 C141.84733,23.9340284 141.686199,24 141.5,24 C141.313801,24 141.15267,23.9340284 141.016602,23.8020833 L136.204102,19.1354167 C136.068033,19.0034716 136,18.8472231 136,18.6666667 C136,18.4861102 136.068033,18.3298618 136.204102,18.1979167 C136.34017,18.0659716 136.501301,18 136.6875,18 L146.3125,18 C146.498699,18 146.65983,18.0659716 146.795898,18.1979167 C146.931967,18.3298618 147,18.4861102 147,18.6666667 Z"></path>
            </g>
          </g>
          <path id="clear" d="M18.6591232,4.98042224 C17.764939,3.44840261 16.551917,2.23538061 15.0198974,1.3411964 C13.4875581,0.447012194 11.8147385,0 10.0003196,0 C8.18606064,0 6.5127615,0.447012194 4.98074188,1.3411964 C3.44856243,2.23522079 2.23554043,3.4482428 1.34135622,4.98042224 C0.447012194,6.51260168 0,8.18590082 0,10 C0,11.814259 0.447172013,13.4872385 1.3411964,15.0195778 C2.23538061,16.5514376 3.44840261,17.7646194 4.98058206,18.6588036 C6.5127615,19.5529878 8.18590082,20 10.0001598,20 C11.8144188,20 13.487718,19.5529878 15.0197376,18.6588036 C16.5517572,17.7647792 17.7647792,16.5515974 18.6589634,15.0195778 C19.5529878,13.4873983 20,11.8140992 20,10 C20,8.185741 19.5529878,6.51244187 18.6591232,4.98042224 L18.6591232,4.98042224 Z M14.7136853,12.3566828 C14.8786179,12.5214556 14.961244,12.7170734 14.961244,12.9427371 C14.961244,13.1770309 14.8786179,13.376804 14.7136853,13.5415768 L13.5418964,14.7135254 C13.3769638,14.8784581 13.1773505,14.9609244 12.9428969,14.9609244 C12.7172332,14.9609244 12.5216154,14.8784581 12.3570024,14.7135254 L10.0001598,12.3566828 L7.64331719,14.7135254 C7.47838456,14.8784581 7.28308641,14.9609244 7.05742277,14.9609244 C6.82296911,14.9609244 6.62335587,14.8784581 6.45842323,14.7135254 L5.28647456,13.5415768 C5.12154193,13.376804 5.03907561,13.1770309 5.03907561,12.9427371 C5.03907561,12.7170734 5.12154193,12.5214556 5.28647456,12.3566828 L7.64331719,10 L5.28647456,7.64315737 C5.12154193,7.47838456 5.03907561,7.2829266 5.03907561,7.05726295 C5.03907561,6.82280929 5.12154193,6.62319605 5.28647456,6.45826341 L6.45842323,5.28647456 C6.62335587,5.12154193 6.82296911,5.03907561 7.05742277,5.03907561 C7.28308641,5.03907561 7.47838456,5.12154193 7.64331719,5.28647456 L10.0001598,7.64315737 L12.3570024,5.28647456 C12.5216154,5.12154193 12.7172332,5.03907561 12.9428969,5.03907561 C13.1773505,5.03907561 13.3769638,5.12154193 13.5418964,5.28647456 L14.7136853,6.45826341 C14.8786179,6.62319605 14.961244,6.82280929 14.961244,7.05726295 C14.961244,7.2829266 14.8786179,7.47838456 14.7136853,7.64315737 L12.3570024,10 L14.7136853,12.3566828 L14.7136853,12.3566828 Z"></path>
          <path id="arrow" d="M19.7938198,1.2019503 L18.7922765,0.200407016 C18.6587538,0.0666384065 18.5050675,0 18.3312174,0 C18.1578592,0 18.0041729,0.0666384065 17.8706502,0.200407016 L9.99699046,8.07382081 L2.12357667,0.200652914 C1.99005396,0.0668843047 1.8363676,0.000245898179 1.66276349,0.000245898179 C1.48915937,0.000245898179 1.33547301,0.0668843047 1.2019503,0.200652914 L0.200407016,1.2021962 C0.0666384065,1.33571891 0,1.48940527 0,1.66300938 C0,1.8366135 0.0668843047,1.99029986 0.200407016,2.12382257 L9.53617728,11.4598387 C9.66969999,11.5933614 9.82338635,11.6599998 9.99699046,11.6599998 C10.1705946,11.6599998 10.324035,11.5933614 10.4575578,11.4598387 L19.7938198,2.12382257 C19.9273425,1.99029986 19.993735,1.8366135 19.993735,1.66300938 C19.993735,1.48940527 19.9273425,1.33571891 19.7938198,1.2019503 L19.7938198,1.2019503 Z"></path>
          <g id="double_arrow">
            <path d="M277.515,135.9L144.464,2.857C142.565,0.955,140.375,0,137.9,0c-2.472,0-4.659,0.955-6.562,2.857l-14.277,14.275
              c-1.903,1.903-2.853,4.089-2.853,6.567c0,2.478,0.95,4.664,2.853,6.567l112.207,112.204L117.062,254.677
              c-1.903,1.903-2.853,4.093-2.853,6.564c0,2.477,0.95,4.667,2.853,6.57l14.277,14.271c1.902,1.905,4.089,2.854,6.562,2.854
              c2.478,0,4.665-0.951,6.563-2.854l133.051-133.044c1.902-1.902,2.851-4.093,2.851-6.567S279.417,137.807,277.515,135.9z"/>
            <path d="M170.732,142.471c0-2.474-0.947-4.665-2.857-6.571L34.833,2.857C32.931,0.955,30.741,0,28.267,0s-4.665,0.955-6.567,2.857
              L7.426,17.133C5.52,19.036,4.57,21.222,4.57,23.7c0,2.478,0.95,4.664,2.856,6.567L119.63,142.471L7.426,254.677
              c-1.906,1.903-2.856,4.093-2.856,6.564c0,2.477,0.95,4.667,2.856,6.57l14.273,14.271c1.903,1.905,4.093,2.854,6.567,2.854
              s4.664-0.951,6.567-2.854l133.042-133.044C169.785,147.136,170.732,144.945,170.732,142.471z"/>
          </g>
        </symbol>
      </svg>
    </div>
  </div>
</template>

<script>

  export default {
    name: 'calendar',
    components: {},
    props: {
      setLimitDays: {
        type: Array,
        default() {
          return ['', '']; // [Start, End]
        }
      },
      setCurrentDays: {
        type: Array,
        default() {
          return ['', '']; // [Start, End]
        }
      },
      datepickerStatus: '',
      placeholderTextStartDay: String,
      placeholderTextEndDay: String,
    },
    data() {
      return {
        activeTypeDay: '', // start || end

        calendarStatus: false,

        weekList: ['日','一','二','三','四','五','六'],
        totalDays: 42,

        startDay: '',
        endDay: '',
        
        defaultStartDay: '',
        defaultEndDay: '',

        year: '',
        month: '',
        date: '',

        lastMonthDays: [],
        currentMonthDays: [],
        nextMonthDays: [],

        weeks: [],
        chunkSize: 7,
      };
    },
    created() {},
    mounted() {
      this.init();
    },
    computed: {
      calendarTitle() {
        return this.startDay && this.activeTypeDay === 'start' ? this.startDay 
            : this.endDay && this.activeTypeDay === 'end' ? this.endDay 
            : 'select date';
      }
    },
    watch: {
      'setLimitDays'() {
        this.setValue();
      },
    },
    methods: {
      setSpecificDay(type) {
        // const specificTime = new Date(this.activeTypeDay === 'start' ? this.defaultStartDay : this.defaultEndDay);
        const specificTime = new Date(
          this.activeTypeDay === 'start' 
          ? this.startDay ? this.startDay : this.defaultStartDay 
          : this.endDay ? this.endDay : this.defaultEndDay);

        this.year = specificTime.getFullYear();
        this.month = specificTime.getMonth();
        this.date = specificTime.getDate();
      },
      // set default / change time format
      setValue() {
        let start = this.setLimitDays[0] ? this.setLimitDays[0].replace(/-/g, "/") : null;
        let end = this.setLimitDays[1] ? this.setLimitDays[1].replace(/-/g, "/") : null;

        this.defaultStartDay = start;
        this.defaultEndDay = end;

        // Using defaultStartDay and defaultEndDay, if no pass setCurrentDays array.
        let currentStart = this.setCurrentDays[0] ? this.setCurrentDays[0].replace(/-/g, "/") : this.defaultStartDay;
        let currentEnd = this.setCurrentDays[1] ? this.setCurrentDays[1].replace(/-/g, "/") : this.defaultEndDay;

        this.startDay = currentStart;
        this.endDay = currentEnd;
      },
      init() {
        this.setValue();
        this.setSpecificDay();
      },
      setDaultStartDayClass(date) {
        let dateGroup = Date.parse(`${date.year}/${date.month}/${date.date}`);
        if (this.defaultStartDay) {
          return Date.parse(this.defaultStartDay) === dateGroup
        }
      },
      setDaultEndDayClass(date) {
        let dateGroup = Date.parse(`${date.year}/${date.month}/${date.date}`);
        if (this.defaultEndDay) {
          return Date.parse(this.defaultEndDay) === dateGroup
        }
      },
      clearDateValue(type) {
        type === 'start' ? (
          this.startDay = this.defaultStartDay
        ) : (
          this.endDay = this.defaultEndDay
        );

        this.$emit('sendDate', {
          startDay: this.defaultStartDay,
          endDay: this.defaultEndDay,
        })
      },
      currentSelectdRange(date) {
        let dateGroup = Date.parse(`${date.year}/${date.month}/${date.date}`);
        let result = false;

        this.startDay && this.endDay ? (
          result = Date.parse(this.startDay) <= dateGroup && Date.parse(this.endDay) >= dateGroup
        ) : null

        return result;
      },
      stopProp(event) {
        event.stopPropagation();
      },

      event() {
        this.calendarStatus = false;
        this.activeTypeDay = '';
        this.$el.removeEventListener('click', this.stopProp);
        document.body.removeEventListener('click', this.event);
      },

      // @type input type => start/end
      toggleCalendar(type) {
        this.calendarStatus = true;

        this.$el.addEventListener('click', this.stopProp);
        document.body.addEventListener('click', this.event);

        this.activeTypeDay = type;

        this.setSpecificDay(type);

        this.clearCalenar();
        this.createCalendar();
      },
      clickDate(ele) {
        const {
          date,
          month,
          year,
          default_range,
        } = ele;

        if (!default_range) return;

        this.activeTypeDay === 'start' ? (
          this.startDay = `${year}/${month}/${date}`
        ) : (
          this.endDay = `${year}/${month}/${date}` 
        );

        this.$emit('sendDate', {
          startDay: this.startDay,
          endDay: this.endDay,
        });
      },
      prevYear(y) {
        let time = new Date();

        this.year = this.year - 1;

        time.setYear(this.year);
        time.setMonth(this.month);

        this.clearCalenar();
        this.createCalendar();
      },
      nextYear(y) {
        let time = new Date();

        this.year = this.year + 1;

        time.setYear(this.year);
        time.setMonth(this.month);

        this.clearCalenar();
        this.createCalendar();
      },
      prevMonth(m) {
        let time = new Date();

        if (this.month) {
          this.month = this.month + m;
        } else {
          this.month = 11;
          this.year = this.year - 1;
        }

        time.setYear(this.year);
        time.setMonth(this.month);

        this.clearCalenar();
        this.createCalendar();
      },
      nextMonth(m) {
        let time = new Date();

        if (this.month < 11) {
          this.month = this.month + m;
        } else {
          this.month = 0;
          this.year = this.year + 1;
        }

        time.setYear(this.year);
        time.setMonth(this.month);

        this.clearCalenar();
        this.createCalendar();
      },
      clearCalenar() {
        this.lastMonthDays = [];
        this.currentMonthDays = [];
        this.nextMonthDays = [];
        this.weeks = [];
      },

      // this.month 一開始沒有 + 1
      // this.month === 0 的時後，要計算成去年和
      createCalendar() {
        let currentFirstDay = new Date(this.year, this.month, 1).getDay();
        let currentMonthDaysLength = new Date(this.year, this.month + 1, 0).getDate();
        
        // 上個月天數
        let lastYear = this.month === 0 ? this.year - 1 : this.year;
        let lastMonth = this.month === 0 ? 12 : this.month;
        let lastDate = null;
        let lastDateGroup = null;

        for(let i = currentFirstDay - 1; i >= 0; i--){

          lastDate = new Date(this.year, this.month, -i).getDate();
          lastDateGroup = Date.parse(`${lastYear}/${lastMonth}/${lastDate}`);

          this.lastMonthDays.push({
            'year': lastYear,
            'month': lastMonth,
            'date': lastDate,
            'prev': true,
            'default_range': (Date.parse(this.defaultStartDay) <= lastDateGroup) && (Date.parse(this.defaultEndDay) >= lastDateGroup),
          })
        }
        
        // 這個月天數
        let currentYear = this.year;
        let currentMonth = this.month + 1;
        let currentDate = null;
        let currentDateGroup = null;

        for(let i = 1; i <= currentMonthDaysLength; i++){

          currentDate = i;
          currentDateGroup = Date.parse(`${currentYear}/${currentMonth}/${currentDate}`);

          this.currentMonthDays.push({
            'year': currentYear,
            'month': currentMonth,
            'date': currentDate,
            'current': true,
            'default_range': (Date.parse(this.defaultStartDay) <= currentDateGroup) && (Date.parse(this.defaultEndDay) >= currentDateGroup),
          })
        }
        
        // 下個月天數
        let nextYear = this.month + 2 === 13 ? this.year + 1 : this.year;
        let nextMonth = this.month + 2 === 13 ? 1 : this.month + 2;
        let nextDate = null;
        let nextDateGroup = null;

        const theRestDays = this.totalDays - this.lastMonthDays.length - this.currentMonthDays.length;
        for (let i = 1; i <= theRestDays; i++) {

          nextDate = i;
          nextDateGroup = Date.parse(`${nextYear}/${nextMonth}/${nextDate}`);

          this.nextMonthDays.push({
            'year': nextYear,
            'month': nextMonth,
            'date': nextDate,
            'default_range': (Date.parse(this.defaultStartDay) <= nextDateGroup) && (Date.parse(this.defaultEndDay) >= nextDateGroup),
          });
        }
        
        const days = this.lastMonthDays.concat(this.currentMonthDays, this.nextMonthDays);
        const daysLength = days.length;
        
        for (let i = 0; i < days.length; i += this.chunkSize) {
          this.weeks.push(days.slice(i, i + this.chunkSize)); 
        }

      },
    },
  };

</script>


<style lang="scss">

.calendar_wrapper {
  position: relative;
  display: inline-flex;
  justify-content: center;
  align-items: center;
  width: auto; 

  // disable datepicker
  &.disable {
    label svg {
      cursor: not-allowed;
    }
    label input {
      background: #EEEEEE; 
      cursor: not-allowed;
      &.active,
      &:focus {
        border-color: #9B9B9B;
      }
    }
  }

  label {
    position: relative;
    display: flex;
    justify-content: flex-start;
    align-items: center;
    input {
      display: inline-flex;
      padding-left: 10px;
      padding-right: 35px;
      width: 100%;
      height: 40px;
      background: #FFFFFF;
      border: 1px solid #9B9B9B;
      border-radius: 4px;
      color: #4A4A4A;
      font-size: 16px;
      &.active {
        border-color: #4DA1FF;
        color: #232425;
      }
      &:focus {
        color: #232425;
      }
    }
  }

  .divider {
    margin: 0 10px;
  }

  svg[type="clear"],
  svg[type="select"] {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
  }

  svg[type="clear"] {
    right: 30px;
    width: 20px;
    height: 20px;
    fill: #C6C6C6;
    
    cursor: pointer;
    &.enabled {
      fill: #757575;
    }
  }

  svg[type="select"] {
    right: 10px;
    width: 11px;
    height: 6px;
    fill: #757575;
    cursor: pointer;
  }
}

.calendar_content {
  opacity: 0;
  visibility: hidden;

  position: absolute;
  top: 100%; 
  left: 0;
  margin: 10px auto;
  padding: 10px;
  background-color: #fff;

  border-radius: 4px;
  box-shadow: 4px 4px 40px 0 rgba(0, 0, 0, .4);
  
  transition: all, .3s;
  z-index: 10000000;
  &.open {
    opacity: 1;
    visibility: visible;
  }
}

.calendar-now-time {
  display: flex;
  
  justify-content: space-between;
  align-items: center;
  margin: 10px 0 20px 0;
  width: 100%;
  font-size: 12px;
  .prev_month { flex: 1; text-align: left; }
  .year { flex: 2; text-align: center; }
  .next_month { flex: 1; text-align: right; }
}

.calendar-action {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 10px 0 20px 0;
  width: 100%;
  .action-move {
    display: flex;
    svg {
      width: 20px;
      height: 20px;
    }

    svg[type="prev"],
    svg[type="next"],
    svg[type="double_prev"],
    svg[type="double_next"] {
      transition: all, .3s;
      &:hover {
        fill: #4DA1FF;
      }
    }
    svg[type="prev"] {
      transform: rotate(90deg);
      &:active {
        transform: rotate(90deg) scale(1.4);
      }
    }

    svg[type="next"] {
      transform: rotate(-90deg);
      &:active {
        transform: rotate(-90deg) scale(1.4);
      }
    }

    svg[type="double_next"] {
      transform: rotate(-180deg);
      &:active {
        transform: rotate(-180deg) scale(1.4);
      }
    }

    &:hover {
      color: #4DA1FF;
    }
    cursor: pointer;
  }
}

.week,
.week_list {
  display: flex;
  cursor: pointer;
}

.week div,
.week_list div {
	display: flex;
	align-items: center;
	justify-content: center;
  margin: 1px;
	padding: 5px;
	width: 30px;
	height: 30px;
  font-size: 12px;
  border: 1px solid transparent;
}

.week div {
  transition: all, .2s;
  &:hover {
    background-color: #4DA1FF;
    color: #ffffff;
  }
  
  &.current_selected_range {
    border: 1px solid #4DA1FF;
    color: #4DA1FF;
    &:hover { color: #ffffff; }
  }
  
  &.out_default_range { color: #DDD; }

  &.default_start_day { border: 1px solid red; background-color: red; color: #ffffff;}
  &.default_end_day { border: 1px solid green; background-color: green; color: #ffffff;}

  &.current_day { font-weight: bold; }
}
</style>
