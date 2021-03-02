<template>
  <div class="calendar-hld">
    <div class="calendar">
      <div class="calendar__table">
        <div class="calendar__table-thead">
          <div class="calendar__table-select-month">
            <div>
              <button
                class="calendar__btn calendar__btn--prev"
                v-on:click="previousMonth"
              >
                <img src="../assets/icons/prev.svg" class="icon" alt="prev" />
              </button>
            </div>
            <div class="calendar__table-month">
              {{ currentMonthAndYear }}
            </div>
            <div>
              <button
                class="calendar__btn calendar__btn--next"
                v-on:click="nextMonth"
              >
                <img src="../assets/icons/next.svg" class="icon" alt="next" />
              </button>
            </div>
          </div>
          <ul class="calendar__table-week-days">
            <li class="calendar__table-week-day">Sun</li>
            <li class="calendar__table-week-day">Mon</li>
            <li class="calendar__table-week-day">Tue</li>
            <li class="calendar__table-week-day">Wen</li>
            <li class="calendar__table-week-day">Thu</li>
            <li class="calendar__table-week-day">Fri</li>
            <li class="calendar__table-week-day">Sat</li>
          </ul>
        </div>
        <div class="calendar__day-list">
          <ul
            v-for="(item, key) in gridArray"
            v-bind:key="key"
            class="calendar__day-list-week"
          >
            <li
              v-for="(date, key) in item"
              v-bind:key="key"
              class="calendar__day"
              :class="dateClass(date)"
            >
              <button
                class="calendar__day-btn "
                v-on:click="setDate(date)"
                v-bind:class="{ 'calendar__day--selected': isActive(date) }"
                :disabled="isDisabled(date)"
              >
                {{ date.getDate() }}
              </button>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import dayjs from "dayjs";
export default {
  props: ["value", "mode", "disabledDays"],
  data() {
    return {
      filterDate: undefined,
      selectedMonth: new Date()
    };
  },
  methods: {
    previousMonth: function() {
      let tmpDate = this.selectedMonth;
      let tmpMonth = tmpDate.getMonth() - 1;
      this.selectedMonth = new Date(tmpDate.setMonth(tmpMonth));
    },
    nextMonth: function() {
      let tmpDate = this.selectedMonth;
      let tmpMonth = tmpDate.getMonth() + 1;
      this.selectedMonth = new Date(tmpDate.setMonth(tmpMonth));
    },
    setDate: function(date) {
      let value = [...(this.value ?? [])];
      let index = this.mode ? 1 : 0;
      value[index] = dayjs(date).format("YYYY-MM-DD");
      this.$emit("input", value);
    },
    isActive: function(date) {
      return date === this.filterDate;
    },
    dateClass: function(date) {
      let time = dayjs(date).format("YYYY-MM-DD");
      let now = dayjs().format("YYYY-MM-DD");
      let checkInTime = this.value[0];
      let checkOutTime = this.value[1];
      let classes = [];
      if (checkInTime && time == checkInTime) {
        classes.push("calendar__day--check-in");
      } else if (checkOutTime && time == checkOutTime) {
        classes.push("calendar__day--check-out");
      } else if (
        checkInTime &&
        checkOutTime &&
        time > checkInTime &&
        time < checkOutTime
      ) {
        classes.push("calendar__day--in-range");
      }
      if (this.disabledDays.includes(time)) {
        classes.push("calendar__day--disabled-day");
      }
      if (this.selectedMonth.getMonth() != date.getMonth()) {
        classes.push("calendar__day--other-month");
      }
      if (now == time) {
        classes.push("calendar__day--today");
      }
      return classes;
    },
    isDisabled: function(date) {
      let time = dayjs(date).format("YYYY-MM-DD");
      let now = dayjs().format("YYYY-MM-DD");
      let checkInTime = this.value[0];
      let checkOutTime = this.value[1];

      if (this.disabledDays.includes(time - 1)) {
        return false;
      }
      if (time < now) {
        return true;
      }
      if (!this.value || this.value.length == 0) {
        return false;
      }
      if (!this.mode) {
        if (checkOutTime) {
          return time > checkOutTime;
        } else {
          return true;
        }
      } else {
        if (checkInTime) {
          return time <= checkInTime;
        } else {
          return true;
        }
      }
    }
  },
  computed: {
    gridArray: function() {
      let calendarMatrix = [];

      let startDay = new Date(
        this.selectedMonth.getFullYear(),
        this.selectedMonth.getMonth(),
        1
      );
      let lastDay = new Date(
        this.selectedMonth.getFullYear(),
        this.selectedMonth.getMonth() + 1,
        0
      );

      let startDow = startDay.getDay() % 7;
      let endDow = lastDay.getDay() % 7;

      startDay.setDate(startDay.getDate() - startDow);
      lastDay.setDate(lastDay.getDate() + (6 - endDow));

      let week = [];
      while (startDay <= lastDay) {
        week.push(new Date(startDay));
        if (week.length === 7) {
          calendarMatrix.push(week);
          week = [];
        }
        startDay.setDate(startDay.getDate() + 1);
      }

      return calendarMatrix;
    },
    formattedDate: function() {
      return this.filterDate ? dayjs(this.filterDate, "YYYY-MM-DD") : "";
    },
    currentMonthAndYear: function() {
      return dayjs(this.selectedMonth).format("MMMM YYYY");
    }
  }
};
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
    cursor: pointer;
    &:focus {
      outline: 0;
      border: 0;
    }
    .icon {
      height: 21px;
      transition: all 0.3s;
    }
    &--prev {
      &:hover {
        .icon {
          transform: translateX(-5px);
        }
      }
    }
    &--next {
      &:hover {
        .icon {
          transform: translateX(5px);
        }
      }
    }
  }
  &__table-week-days {
    display: flex;
    justify-content: space-around;
    align-items: center;
    height: 60px;
    list-style: none;
    padding: 0 20px;
    border: 1px solid #d9d9d9;
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
    border: 1px solid #d9d9d9;
    border-top: 0;
    padding-bottom: 10px;
  }
  &__day {
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 100%;
    height: 40px;
    width: 40px;
    cursor: pointer;
    &--today {
      border: 1px solid #00dbb1;
    }
    &--check-in,
    &--check-out {
      background-color: #00dbb1;
      position: relative;
      z-index: 1;
      .calendar__day-btn {
        color: #fff !important;
      }
    }
    &--in-range {
      border-radius: 0;
      position: relative;
      &:before {
        content: "";
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
        z-index: 9;
        font-weight: 100;
      }
    }
    &--other-month {
      .calendar__day-btn {
        color: #d9d9d9;
        &:hover {
          color: #d9d9d9;
          background: transparent;
        }
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
    border-radius: 50%;
    cursor: pointer;
    color: #a7a7a7;
    &:disabled {
      color: #ccc;
      &:hover {
        color: #ccc;
        background: transparent;
        cursor: auto;
      }
    }
    &:focus {
      outline: 0;
      border: 0;
    }
    &:hover {
      background: #00dbb1;
      color: #fff;
    }
  }
}
</style>
