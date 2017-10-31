<template>
  <div for="" class="calendar_wrapper">
    <label>
      <input 
        :placeholder="placeholderTextStartDay || '起始日期'"
        :class="{ active: activeTypeDay === 'start' }"
        v-model="startDay"
        type="text" 
        readonly="readonly"
        @click="toggleCalendar('start')">
        <svg v-if="startDay" :class="{ enabled: startDay }" type="clear" viewBox="0 0 20 20" @click="clearDateValue('start')"><use xlink:href="#clear"></use></svg>
        <svg type="select" viewBox="0 0 11 6"><use xlink:href="#select"></use></svg>
    </label>
    <span> ~ </span>
    <label>
      <input 
        :placeholder="placeholderTextEndDay || '結束日期'"
        :class="{ active: activeTypeDay === 'end' }"
        v-model="endDay"
        type="text" 
        readonly="readonly"
        @click="toggleCalendar('end')">
        <svg v-if="endDay" :class="{ enabled: endDay }" type="clear" viewBox="0 0 20 20" @click="clearDateValue('end')"><use xlink:href="#clear"></use></svg>
        <svg type="select" viewBox="0 0 11 6"><use xlink:href="#select"></use></svg>
    </label>
    <div class="calendar_content" :class="{ open: calendarStatus }">
      <div class="calendar-now-time">
        <div class="prev_month">{{ month === 0 ? 12 : month }}月</div>
        <div class="year">{{ year }}年 / {{ month + 1 }}月</div>
        <div class="prev_month">{{ month + 2 === 13 ? 1 : month + 2 }}月</div>
      </div>
      <div class="calendar-action">
        <div class="action-move" @click="prevMonth(-1)">
          <div><svg type="prev" viewBox="0 0 20 12"><use xlink:href="#arrow"></use></svg></div>
        </div>
        <div class="action-text">{{ calendarTitle }}</div>
        <div class="action-move" @click="nextMonth(1)">
          <div><svg type="next" viewBox="0 0 20 12"><use xlink:href="#arrow"></use></svg></div>
        </div>
      </div>
      <div class="week_wrapper">
        <div class="week_list">
          <div class="" v-for="(list, z) in weekList" :key="z">{{ list }}</div>
        </div>
        <div class="week" v-for="(w, i) in weeks" :key="i">
          <!-- out_range: Date.parse(`${d.year}/${d.month}/${d.date}`) > Date.parse(new Date()), -->
          <div 
          @click="outRange(d)" 
          v-for="(d, j) in w" 
          :key="j" 
          :class="{ 
            current_range: currentRange(d),
            default_start_day: setDaultStartDay(d),
            default_end_day: setDaultEndDay(d),
            if_startDay_selected: Date.parse(endDay) < Date.parse(`${d.year}/${d.month}/${d.date}`) && activeTypeDay === 'start',
            if_endDay_selected: Date.parse(startDay) > Date.parse(`${d.year}/${d.month}/${d.date}`) && activeTypeDay === 'end',
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
      setDays: {
        type: Array,
        default() {
          return ['2001/3/20', '2018/3/20']; // [Start, End]
        }
      },
      placeholderTextStartDay: ['String'],
      placeholderTextEndDay: ['String'],
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

        preMonthDays: [],
        currentMonthDays: [],
        nextMontDays: [],
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
            : '請挑選日期';
      }
    },
    watch: {
      'setDays'() {
        this.setData();
      },
    },
    methods: {
      setSpecificDay(type) {
        const specificTime = new Date(this.activeTypeDay === 'start' ? this.setDays[0] : this.setDays[1]);

        this.year = specificTime.getFullYear();
        this.month = specificTime.getMonth();
        this.date = specificTime.getDate();

        this.clearCalenar();
        this.createCalendar();
      },
      setData() {
        this.defaultStartDay = this.setDays[0];
        this.defaultEndDay = this.setDays[1];
        this.startDay = this.setDays[0];
        this.endDay = this.setDays[1];
      },
      init() {
        this.setData();
        this.setSpecificDay();
      },
      setDaultStartDay(date) {
        let dateGroup = Date.parse(`${date.year}/${date.month}/${date.date}`);
        if (this.defaultStartDay) {
          return Date.parse(this.defaultStartDay) === dateGroup
        }
      },
      setDaultEndDay(date) {
        let dateGroup = Date.parse(`${date.year}/${date.month}/${date.date}`);
        if (this.defaultEndDay) {
          return Date.parse(this.defaultEndDay) === dateGroup
        }
      },
      clearDateValue(type) {
        type === 'start' ? (
          this.startDay = this.defaultStartDay,
          this.$emit('sendDate', {
            startDay: '',
            endDay: this.defaultEndDay,
          })
        ) : (
          this.endDay = this.defaultEndDay,
          this.$emit('sendDate', {
            startDay: this.defaultStartDay,
            endDay: '',
          })
        );
      },
      currentRange(date) {
        let dateGroup = Date.parse(`${date.year}/${date.month}/${date.date}`);
        let result = null;

        this.startDay && this.endDay ? (
          result = Date.parse(this.startDay) <= dateGroup && dateGroup <= Date.parse(this.endDay)
        ) : this.startDay ? (
          result = Date.parse(this.startDay) <= dateGroup && dateGroup <= Date.parse(new Date())
        ) : this.endDay ? (
          result = dateGroup <= Date.parse(this.endDay)
        ) : (
          // 預設一開始從今天的日期開始全選
          result = dateGroup <= Date.parse(new Date())
        )

        return result;
      },
      outRange(date) {
        let dateGroup = Date.parse(`${date.year}/${date.month}/${date.date}`);

        // 條件為 this.startDay < dateGroup < this.endDay 這區間內
        (Date.parse(this.defaultStartDay) <= dateGroup) && (Date.parse(this.defaultEndDay) >= dateGroup) ? this.clickDate(date) : null;
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
      },
      clickDate(ele) {
        const {
          date,
          month,
          year,
        } = ele;

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
        this.preMonthDays = [];
        this.currentMonthDays = [];
        this.nextMontDays = [];
        this.weeks = [];
      },
      // this.month 一開始沒有 + 1
      // this.month === 0 的時後，要計算成去年和
      createCalendar() {
        let currentFirstDay = new Date(this.year, this.month, 1).getDay();
        let currentMonthDaysLength = new Date(this.year, this.month + 1, 0).getDate();
      
        // 上個月天數
        for(let i = currentFirstDay - 1; i >= 0; i--){
          this.preMonthDays.push({
            'year': this.month === 0 ? this.year - 1 : this.year,
            'month': this.month === 0 ? 12 : this.month,
            'date': new Date(this.year, this.month, -i).getDate(),
            'prev': true,
          })
        }
        
        // 這個月天數
        for(let i = 1; i <= currentMonthDaysLength; i++){
          this.currentMonthDays.push({
            'year': this.year,
            'month': this.month + 1,
            'date': i,
            'current': true,
          })
        }
        
        // 下個月天數
        const theRestDays = this.totalDays - this.preMonthDays.length - this.currentMonthDays.length;

        for (let i = 1; i <= theRestDays; i++) {
          this.nextMontDays.push({
            'year': this.month + 2 === 13 ? this.year + 1 : this.year,
            'month': this.month + 2 === 13 ? 1 : this.month + 2,
            'date': i,
          });
        }
        
        const days = this.preMonthDays.concat(this.currentMonthDays, this.nextMontDays);
        const daysLength = days.length;
        
        for (let i = 0; i < days.length; i += this.chunkSize) {
          this.weeks.push(days.slice(i, i + this.chunkSize)); 
        }

      },
    },
  };

</script>


<style lang="scss">

label {
  position: relative;
  display: flex;
  justify-content: flex-start;
  align-items: center;
  // 有按鈕時
  &.have_clear_btn {
    svg[type="clear"] {
      right: 70px;
    }
    input {
      border-top-right-radius: 0;
      border-bottom-right-radius: 0;
      border-right: none; 
    }
  }

  svg[type="clear"] {
    position: absolute;
    top: 50%;
    right: 10px;
    width: 20px;
    height: 20px;
    fill: #C6C6C6;
    transform: translateY(-50%);
    cursor: pointer;
    &.enabled {
      fill: #757575;
    }
  }

  svg[type="select"] {
    width: 11px;
    height: 6px;
    fill: #757575;
    cursor: pointer;
  }

  input {
    padding-left: 10px;
    padding-right: 35px;
    width: 100%;
    height: 40px;
    background: #FFFFFF;
    border: 1px solid #9B9B9B;
    border-radius: 4px;
    color: #4A4A4A;
    font-size: 16px;
    &:focus {
      color: #232425;
    }
  }
  
  .clearbtn {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 85px;
    height: 40px;
    background: #228BFF;
    font-size: 16px;
    color: rgba(255,255,255,0.87);
    border: 1px solid #9B9B9B;
    border-left: none;
    border-top-right-radius: 4px;
    border-bottom-right-radius: 4px;
    cursor: pointer;
  }
}

.input_dropdown {
  position: relative;
  display: inline-flex;
  z-index: z('input');
  .up {
    position: absolute;
    top: 50%;
    right: 10px;
    transform: translateY(-50%);
  }
  
  .input_dropdown_select,
  .input_dropdown_search {
    position: absolute;
    padding-top: 44px;
    top: 0;
    left: 0;
    width: 100%;
    height: auto;   
  }

  .input_dropdown_select_wrapper,
  .input_dropdown_search_wrapper {
    height: auto;
    background: #FFFFFF;
    border: 1px solid #9B9B9B;
    border-radius: 4px;
    color: #9B9B9B;
    font-size: 16px;
  }

  .input_dropdown_search .input_dropdown_search_wrapper {
    padding: 3px;
  }

  .input_dropdown_search .input_dropdown_search_wrapper .input_dropdown_search_content {
    overflow: auto;
    position: relative;
    max-height: 400px;
    input {
      position: fixed;
      top: 3px;
      left: 3px;
      right: 3px;
    }
  }

  .input_dropdown_select .input_dropdown_select_wrapper .input_dropdown_select_item,
  .input_dropdown_search .input_dropdown_search_wrapper .input_dropdown_search_item {
    padding: 10px;
    transition: all, .3s;
    cursor: pointer;
    &:last-child {
      border-bottom-left-radius: 4px;
      border-bottom-right-radius: 4px;
    }
    &.active,
    &:hover {
      background-color: #f3f3f3;
    }
  }
}


.calendar_wrapper {
  position: relative;
  display: inline-flex;
  justify-content: center;
  align-items: center;
  width: auto; 
  label {
    position: relative;
  }
  span {
    margin-right: 10px;
  }
  input { 
    display: inline-flex;
    &.active {
      border-color: #4DA1FF;
      color: #232425;
    }
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
  margin: 10px 0 20px 0;
  padding: 0 5px;
  width: 100%;
  justify-content: space-between;
  align-items: center;
  font-size: 12px;
}

.calendar-action {
  display: flex;
  margin: 10px 0;
  padding: 0 5px;
  width: 100%;
  justify-content: space-between;
  align-items: center;
  .action-move {
    svg {
      width: 20px;
      height: 20px;
    }
    svg[type="prev"],
    svg[type="next"] {
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
}

.week div {
  transition: all, .2s;
  &:hover {
    border: none;
    background-color: #4DA1FF;
    color: #ffffff;
  }
  &.current_range {
    border: 1px solid #4DA1FF;
    color: #4DA1FF;
    &:hover {
      color: #ffffff;
    }
  }

  &.default_start_day { border: 1px solid red; color: red;}
  &.default_end_day { border: 1px solid green; color: green;}

  &.if_startDay_selected,
  &.if_endDay_selected,
  &.out_range {
    color: #DDD;
  }
  &.current_day {
    font-weight: bold;
  }
}
</style>
