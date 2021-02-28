<template>
  <div class="calendar-hld">
      <div class="calendar">
        <div class="calendar__table">
          <div class="calendar__table-thead">
            <div class="calendar__table-select-month">
              <div>
                <button class="calendar__btn calendar__btn--prev" v-on:click="previousMonth">
                  <img src="../assets/icons/prev.svg" class="icon" alt="prev">
                  </button>
              </div>
              <div class="calendar__table-month">
                {{currentMonthAndYear}}
              </div>
              <div>
                <button class="calendar__btn calendar__btn--next" v-on:click="nextMonth">
                  <img src="../assets/icons/next.svg" class="icon" alt="next">
                  </button>
              </div>
            </div>
            <ul class="calendar__table-week-days">
              <li class="calendar__table-week-day">Mon</li>
              <li class="calendar__table-week-day">Tue</li>
              <li class="calendar__table-week-day">Wen</li>
              <li class="calendar__table-week-day">Thu</li>
              <li class="calendar__table-week-day">Fri</li>
              <li class="calendar__table-week-day">Sat</li>
              <li class="calendar__table-week-day">Sun</li>
            </ul>
          </div>
          <div class="calendar__day-list" data-bind="foreach:gridArray">
            <ul v-for="(item, key) in gridArray" v-bind:key="key" class="calendar__day-list-week">
              <li v-for="(date, key) in item" v-bind:key="key" class="calendar__day" :class="dateClass(date)">
                <button class="calendar__day-btn " v-on:click="setDate(date)" v-bind:class="{'day-selected':isActive(date)}" :disabled="isDisabled(date)">
                    {{date.getDate()}}
                </button>
              </li>
            </ul>
          </div>
        </div>
      </div>



  </div>
</template>

<script>
import dayjs from 'dayjs';
export default {
  props: ['value', 'mode', 'disabledDays'],
data() {
    return {
      filterDate: undefined,
      selectedMonth: new Date(),
    }
  },
  methods: {
    previousMonth: function() {
      let tmpDate = this.selectedMonth;
      let tmpMonth = tmpDate.getMonth() - 1;
      this.selectedMonth = new Date(tmpDate.setMonth(tmpMonth));
      // this.currentMonthAndYear = dayjs(this.selectedMonth, 'YYYY-MM-DD');
    },
    nextMonth: function() {
      let tmpDate = this.selectedMonth;
      let tmpMonth = tmpDate.getMonth() + 1;
      this.selectedMonth = new Date(tmpDate.setMonth(tmpMonth));
      // this.currentMonthAndYear = dayjs(this.selectedMonth, 'YYYY-MM-DD');
    },
    setDate: function(date) {
      // if (date == this.filterDate) {
      //   console.log('setting undefined');
      //   // this.filterDate = undefined;
      //   //unselected
      // } else {
      //   this.filterDate = date;
      // }
      let value = [...(this.value ?? [])];
      let index = this.mode ? 1 : 0;
      value[index] = dayjs(date).format('YYYY-MM-DD');
      this.$emit('input', value);
    },
    isActive: function(date) {
      return date === this.filterDate;
    },
    getCalendarMatrix: function(date) {
      let calendarMatrix = []

      let startDay = new Date(date.getFullYear(), date.getMonth(), 1)
      let lastDay = new Date(date.getFullYear(), date.getMonth() + 1, 0)

      // Modify the result of getDay so that we treat Monday = 0 instead of Sunday = 0
      let startDow = (startDay.getDay() + 6) % 7;
      let endDow = (lastDay.getDay() + 6) % 7;

      // If the month didn't start on a Monday, start from the last Monday of the previous month
      startDay.setDate(startDay.getDate() - startDow);

      // If the month didn't end on a Sunday, end on the following Sunday in the next month
      lastDay.setDate(lastDay.getDate() + (6 - endDow));

      let week = []
      while (startDay <= lastDay) {
        week.push(new Date(startDay));
        if (week.length === 7) {
          calendarMatrix.push(week);
          week = []
        }
        startDay.setDate(startDay.getDate() + 1)
      }

      return calendarMatrix;
    },
    dateClass: function(date) {
      let time = dayjs(date).format('YYYY-MM-DD');
      let now = dayjs().format('YYYY-MM-DD');
      let checkInTime = this.value[0];
      let checkOutTime = this.value[1];
      let classes = [];
      if(checkInTime && time == checkInTime) {
        classes.push('check-in')
      } else if(checkOutTime && time == checkOutTime) {
        classes.push('check-out')
      } else if((checkInTime && checkOutTime) && (time > checkInTime && time < checkOutTime)) {
        classes.push('in-range')
      }
      if(this.disabledDays.includes(time)) {
        classes.push('disabled-day')
      }
      if(now == time) {
        classes.push('today');
      }
      return classes;
    },
    isDisabled: function (date) {
      let time = dayjs(date).format('YYYY-MM-DD');
      let now = dayjs().format('YYYY-MM-DD');
      console.log(time);
      console.log(now);
      let checkInTime = this.value[0];
      let checkOutTime = this.value[1];

      if(this.disabledDays.includes(time)) {
        return true;
      }


      if(time < now) {
        return true;
      }
      if(!this.value || this.value.length == 0){
        return false;
      }
      if(!this.mode) {
        if(checkOutTime) {
          return time > checkOutTime
        } else {
          return true
        }
      } else {
        if(checkInTime) {
          return time <= checkInTime 
        } else {
          return true
        }
      }
    }
  },
  computed: {
    // a computed getter
    gridArray: function() {
      let grid = this.getCalendarMatrix(this.selectedMonth);
      return grid;
    },
    formattedDate: function() {
      return this.filterDate ? dayjs(this.filterDate,'YYYY-MM-DD') : '';
    },
    currentMonthAndYear: function() {
      return dayjs(this.selectedMonth).format('MMMM YYYY');
    }

  }
}
</script>

<style lang="scss" scoped>
.calendar {
  position: absolute;
  top: 105%;
  right: 0;
  left: 0;
  color: rgb(214, 213, 213);
  background-color: #fff;
  &__table {
    width: 100%;
    border-spacing: 0;
  }
  &__table-select-month {
    display: flex;
    justify-content: space-around;
    align-items: center;
    background-color: #00dbb1;
    color: #fff;
    height: 60px;
  }
  &__table-month {
    font-size: 21px;
    font-weight: 300;
    text-align: center;
  }
  &__btn {
    border: 0;
    background-color: transparent;
    box-shadow: none;
    &:focus {
      outline: 0;
      border: 0;
    }
    .icon {
      height: 21px;
    }
  }
  &__table-week-days {
    display: flex;
    justify-content: space-around;
    align-items: center;
    height: 60px;
    list-style: none;
    padding: 0 20px;
    border: 1px solid #D9D9D9;
    border-bottom: 0;
  }
  &__table-week-day {
    font-size: 12px;
    width: 40px;
    text-align: center;
    font-weight: 600;
  }
  &__day-list-week {
    display: flex;
    justify-content: space-around;
    align-items: center;
    list-style: none;
    padding: 0 20px;
  }
  &__day-list {
    border: 1px solid #D9D9D9;
    border-top: 0;
    padding-bottom: 10px
  }
  &__day {
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 100%;
    height: 40px;
    width: 40px;
    &.today {
      border: 1px solid #00dbb1;
    }
    &.check-in,
    &.check-out {
      background-color: #00dbb1;
      position: relative;
      z-index: 1;
      .calendar__day-btn { 
        color: #fff;
      }
    }
    &.in-range {
      border-radius: 0;
      position: relative;
      &:before {
        content: '';
        position: absolute;
        top: 2px;
        right: -15px;
        bottom: 2px;
        left: -15px;
        background-color: #55fff6;
        z-index: 0;
      }
        .calendar__day-btn { 
          color: #00dbb1;
          position: relative;
          z-index: 99999;
      }
    }
  }
  &__day-btn {
    box-shadow: none;
    border: 0;
    background: none;
    font-weight: 600;
    height: 100%;
    width: 100%;
    color: #a7a7a7;
    &:disabled {
      color: #ccc;
    }
    &:focus {
      outline: 0;
      border: 0;
    }
  }
}


</style>