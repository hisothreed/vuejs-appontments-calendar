<template>
  <div class="root-container">
    <div class="header-container uk-margin">
      <!-- filters -->
      <div class="range-selector uk-flex uk-flex-row uk-flex-wrap uk-flex-center">
        <div class="uk-width-1-5 uk-flex-row uk-flex uk-flex-center">
          <span class="uk-width">
            Time gap
          </span>
          <select class="uk-select" v-model="increment_value">
            <option value="15">
              15
            </option>
            <option value="30">
              30
            </option>
            <option value="60">
              60
            </option>
          </select>
        </div>
        <div class="uk-button-group" style="margin-left: 20px;">
          <!-- <button @click="mode = 'day'" class="uk-button uk-button-default">
            Day
          </button> -->
          <button @click="mode = 'week'" class="uk-button uk-button-default">
            Week
          </button>
          <button @click="mode = 'month'" class="uk-button uk-button-default">
            Month
          </button>
          <!-- <button @click="mode = 'year'" class="uk-button uk-button-default">
            year
          </button> -->
        </div>
        <div class="uk-width uk-flex uk-flex-center uk-margin">
          <div class="uk-slidenav-container">
            <a 
            :uk-tooltip="`Previous ${mode}`"
            @click="decrementOffset()" 
            uk-slidenav-previous></a>
            <span v-if="mode === 'week'">
              {{ currentWeek }}
            </span>
            <span v-if="mode === 'month'">
              {{ currentMonth }}
            </span>
            <a 
            :uk-tooltip="`Next ${mode}`"
            @click="incrementOffset()"
            uk-slidenav-next></a>
          </div>
        </div>
      </div>
    </div>
    <!-- content -->
    <!-- mode week -->
    <div v-if="mode === 'week'" class="container">
      <div class="container-grid uk-flex uk-flex-column uk-flex-left">
        <!-- days -->
        <div class="uk-flex uk-flex-row days-presenter">
          <div class="">
            <span></span>
          </div>
          <div 
          v-for="day in days" 
          :key="day.vaue" 
          class="uk-width uk-text-left uk-padding-small">
            <span class="uk-text-small">{{ day.value }}</span>
            <span class="uk-text-uppercase uk-text-big">{{ day.name }}</span>
          </div>
        </div>
        <!-- content -->
        <div class="presenter-container uk-flex uk-flex-row uk-width">
          <!-- now pointer -->
          <div 
          class="now-line-container" 
          @mouseover="showNowDot = true" 
          @mouseleave="showNowDot = false" 
          :style="lineStyle">
            <div v-show="showNowDot" :uk-tooltip="`title: ${now};`">
            </div>
          </div>
          <!-- times -->
          <div class="times-contaienr uk-flex uk-flex-column uk-align-left">
            <div v-for="time in times" :key="time.value" class="time-container">
              {{ time.name }}
            </div>
          </div>
          <!-- time-ground -->
          <div v-for="day in days" :key="day.value" class="uk-width" style="position: relative;height: 100%;">
            <ul class="uk-list uk-width ground-list">
              <li 
              class="ground-item"
              v-for="time in times"
              :key="time.value">
              </li>
            </ul>
            <!-- slots of day-->
            <div class="slots-container uk-width" v-if="schedules[day.value]">
              <TimeSlot
              :mode="mode"
              v-for="slot in schedules[day.value].slots"
              :key="slot.id"
              :increment="increment_value"
              :timeSlot="slot"
              :schedule="schedules[day.value]" />
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- mode end -->
    <!-- mode month -->
    <div v-if="mode === 'month'" class="container">
      <div class="container-grid uk-flex uk-flex-column uk-flex-left">
        <!-- days -->
        <div class="uk-flex uk-flex-row days-presenter">
          <div 
          v-for="day in days" 
          :key="day.value" 
          class="uk-width uk-text-left uk-padding-small">
            <span class="uk-text-uppercase uk-text-big">{{ day.name }}</span>
          </div>
        </div>
        <!-- content -->
        <div class="presenter-container uk-flex uk-flex-column uk-width">
          <!-- grid -->
          <!-- row -->
          <div class="uk-flex uk-flex-row uk-flex-wrap month-grid-row">
            <!-- columns -->
            <div
            v-for="day in daysOfMonth"
            :key="day.value"
            class="uk-text-left uk-padding-small month-grid-cell">
              <span class="uk-text-small">
                {{ day.value }}
              </span>
              <ul class="uk-list" v-if="schedules[day.value] && schedules[day.value].slots"> 
                <li v-for="slot in schedules[day.value].slots" :key="slot.id">
                  <TimeSlot
                  :mode="mode"
                  :timeSlot="slot"
                  :schedule="schedules[day.value]" />
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>  
</template>
<script>
import moment from 'moment';
import TimeSlot from './TimeSlot';
export default {
  components: {
    TimeSlot
  },
  data() {
    return {
      increment_value: '15',
      showNowDot: false,
      mode: 'week',
      settings: {
        week_start_offset: 0,
        week_end_offset: 1,
        week_start_time: {},
        week_end_time: {},
      },
      schedules: {
        '13/4': {
          offset: 3,
          id: 1,
          slots: [
            {              
              id: 1,
              start_time: '02:00',
              end_time: '03:00',
              range: 1,
              status: 'blocked',
            },
            {  
              id: 2,
              start_time: '03:00',
              end_time: '04:00',
              range: 1,
              status: 'taken',
            },
            {
              id: 3,
              start_time: '04:00',
              end_time: '05:00',
              range: 1,
              status: 'taken',
            },
            {
              id: 4,
              start_time: '05:00',
              end_time: '08:00',
              range: 3,
              status: 'free',
            },
          ]
        },
        '14/4': {
          offset: 3,
          id: 1,
          slots: [
            {              
              id: 1,
              start_time: '02:00',
              end_time: '03:00',
              range: 1,
              status: 'blocked',
            },
            {  
              id: 2,
              start_time: '03:00',
              end_time: '04:00',
              range: 1,
              status: 'taken',
            },
            {  
              id: 2,
              start_time: '03:00',
              end_time: '03:30',
              range: 0.5,
              status: 'free',
            },
            {
              id: 3,
              start_time: '03:30',
              end_time: '04:15',
              range: 0.85,
              status: 'blocked',
            },
            {
              id: 4,
              start_time: '04:14',
              end_time: '05:00',
              range: 0.86,
              status: 'free',
            },
          ]
        },
        '17/4': {
          offset: 3,
          id: 1,
          slots: [
            {              
              id: 1,
              start_time: '01:30',
              end_time: '02:45',
              range: 1.5,
              status: 'blocked',
            },
            {  
              id: 2,
              start_time: '02:45',
              end_time: '04:00',
              range: 1.55,
              status: 'taken',
            },
            {
              id: 3,
              start_time: '04:00',
              end_time: '05:00',
              range: 1,
              status: 'taken',
            },
            {
              id: 4,
              start_time: '05:00',
              end_time: '08:00',
              range: 3,
              status: 'free',
            },
          ]
        },
      }, 
    }
  },
  computed: {
    times() {
      var arr = [];
      for (let h = 0; h < 24; h++) {
        for (let m = 0; m < 60; m+= (+this.increment_value)) {
          arr.push(
            { 
              value: `${h}:${m}`,
              name: moment().hours(h).minutes(m).format('hh:mm a')
            }
          );
        }
      }
      return arr;
    },
    days() {
      var arr = [];
      for (var i = 0; i < 7; i++) {
        arr.push({ 
          value: moment().add(this.settings.week_start_offset, 'weeks').add(i, 'days').format('DD/M'),
          name: moment().add(i, 'days').format('dddd'),
        });
      }
      return arr;
    },
    daysOfMonth() {
      var arr = [];
      let month = moment().add(this.settings.week_start_offset, 'weeks').month();
      let daysInMonth = moment().add(this.settings.week_start_offset, 'weeks').daysInMonth();
      for (var i = 0; i < daysInMonth; i++) {
        arr.push({
          value: moment().month(month).date(1).add(i, 'days').format('DD/M'),
          name: moment().month(month).date(1).add(i, 'days').format('dddd'),
        });
      }
      return arr;
    },
    lineStyle() {
      return {
        'top': `${(+moment().format('H') * ((15 / +this.increment_value) * 200) + ((+moment().format('m') / this.increment_value) * 50))}px`
      }
    },
    now() {
      let v =  (moment().format('hh:mm a'))
      return `${v}`;
    },
    currentWeek() {
      if (this.settings.week_start_time.format('MMM') && this.settings.week_start_time.format('MMM') === this.settings.week_end_time.format('MMM')) {
        return `${this.settings.week_start_time.format('MMMM')}`;
      } else {
        return `${this.settings.week_start_time.format('MMMM')} - ${this.settings.week_end_time.format('MMM')}`;
      }
    },
    currentMonth() {
      return `${this.settings.week_start_time.format('MMMM')}`;
    }
  },
  watch: {
    'settings.week_start_offset': {
      handler() {
        this.settings.week_start_time = this.moment().add(this.settings.week_start_offset, 'weeks');
        this.settings.week_end_time = this.moment().add(this.settings.week_end_offset, 'weeks');
      }
    }
  },
  created() {
    this.moment = moment;
    this.settings.week_start_time = moment().add(this.settings.week_start_offset, 'weeks');
    this.settings.week_end_time = moment().add(this.settings.week_end_offset, 'weeks');
  },
  methods: {
    decrementOffset() {
      if (this.mode === 'week') { 
        this.settings.week_start_offset -= 1; 
        this.settings.week_end_offset -= 1;
      } if (this.mode === 'month') {
        this.settings.week_start_offset -= 4; 
        this.settings.week_end_offset -= 4;
      }
    },
    incrementOffset() {
      if (this.mode === 'week') {
        this.settings.week_start_offset += 1; 
        this.settings.week_end_offset += 1;
      } 
      if (this.mode === 'month') {
        this.settings.week_start_offset += 4; 
        this.settings.week_end_offset += 4;
      }
    }
  }
}
</script>
<style>
  * {
    box-sizing: border-box;
  }
  .uk-list li{
    margin: 0 !important;
  }
  .uk-list ul {
    padding: 0 !important;
  }
  .root-container {
    background-color: whitesmoke;
    padding: 20px;
  }
  .days-presenter {
    box-shadow: 5px 5px 20px rgba(0,0,0,0.1);
    z-index: 10;
  }
  .days-presenter > div:first-child:not(.uk-width) {
    min-width: 60px !important;
    background-color: none;
  }
  .days-presenter div {
    height: 70px;
    background-color: white;
    border-right: 1px solid rgba(0,0,0,0.1);
    min-width: 150px;
    align-content: center;
    justify-content: center;
    display: flex;
    flex-flow: column;
  }
  .month-grid-row {
    min-height: 150px;
    max-width: 100%;
    border-left: 1px solid rgba(0,0,0,0.1);
    border-bottom: 1px solid rgba(0,0,0,0.1);
  }
  .month-grid-cell {
    overflow-y: auto;
    height: 150px;
    width: calc(100% / 7);
    border-bottom: 1px solid rgba(0,0,0,0.1);
    border-right: 1px solid rgba(0,0,0,0.1);
    background-color: white;
  }
  .presenter-container {
    max-height: 600px;
    position: relative;
    overflow-y: scroll;
  }
  .now-line-container {
    height: 1px;
    z-index: 100;
    width: 100%;
    background-color: red;
    position: absolute;
  }
  .now-line-container div {
    background-color: red;
    height: 8px;
    width: 8px;
    border-radius: 100%;
    position: absolute;
    left: 50px;
    bottom: -4px;
  }
  .times-contaienr {
    min-width: 60px;
    height: 100%;
    box-shadow: 5px 5px 20px rgba(0,0,0,0.1);
    z-index: 10;
    background-color: white;
    margin: 0px !important;
  }
  .time-container {
    height: 50px;
    text-align: right;
    justify-content: top;
    display: flex;
    flex-flow: column;
    padding-right: 5px;
    border-bottom: 1px solid rgba(0,0,0,0.1);
    font-size: 0.8rem;
  }
  .ground-list {
    background-color: white;
    height: 100%;
    width: 100%;
  }
  .ground-item {
    height: 50px;
    border-bottom: 1px solid rgba(0,0,0,0.1);
    border-right: 1px solid rgba(0,0,0,0.1);
  }
  .slots-container {
    position: absolute;
    top: 0;
    height: 100%;
    width: 100%;
  }
  .header-item {
    min-height: 80px;
    flex: auto;
    display: flex;
    flex-flow: column;
    align-content: middle;
    justify-content: center;
    border: 1px solid rgba(0,0,0,0.1);
  }
</style>


