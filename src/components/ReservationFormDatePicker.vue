<template>
    <div class="date-picker">
        <p class="date-picker__title">Dates</p>
        <div class="date-picker__row">
            <button class="date-picker__btn" type="button" :class="(!mode && calendarVisible) ? 'active' : ''" @click="selectMode(false)">{{ checkIn }}</button>
            <img src="../assets/icons/arrow.svg" alt="arrow" class="date-picker__arrow">
            <button class="date-picker__btn" type="button" :class="(mode && calendarVisible)  ? 'active' : ''" @click="selectMode(true)" :disabled="!mode">{{ checkOut }}</button>
        </div>
        <ReservationFormCalendar v-if="calendarVisible" @input="selected" v-model="dateRange" :mode="mode" :disabledDays="['2021-05-01', '2021-05-03', '2021-12-24']"></ReservationFormCalendar>
    </div>
</template>

<script>
import dayjs from 'dayjs';
import ReservationFormCalendar from './ReservationFormCalendar.vue'
export default {
    components: {
        ReservationFormCalendar
    },
    data() {
        return {
            mode: false,
            dateRange: [],
            calendarVisible: false,
        }
    },
    methods: {
        selected: function() {
            if(!this.mode) {
                this.mode = true;
            } else {
                this.calendarVisible = false;
            }
        },
        selectMode: function (mode) {
            this.calendarVisible = true;
            this.mode = mode;
        }
    },
    computed: {
        checkIn: function() {
            return this.dateRange[0] ? dayjs(this.dateRange[0]).format('DD-MM-YYYY') : 'Check In'
        },
        checkOut: function() {
            return this.dateRange[1] ? dayjs(this.dateRange[1]).format('DD-MM-YYYY') : 'Check Out'
        }
    }
}
</script>

<style scoped lang="scss">
    .date-picker {
        position: relative;
        padding: 28px 0 0;
        &__title {
            font-size: 11px;
            font-weight: 700;
        }
        &__row {
            display: flex;
            border: 1px solid #D9D9D9;
            border-radius: 1px;
            margin-top: 10px;
            padding: 11px;
        }
        &__btn {
            display: block;
            padding: 5px 25px 5px 8px;
            border: 0;
            background: transparent;
            box-shadow: none;
            cursor: pointer;
            font-size: 18px;
            &:focus {
                outline: none;
            }
            &:hover {
                background-color: #d9d9d9D9;
            }
            &.active {
                background-color: #d9d9d9D9;
            }
        }
        &__arrow {
            width: 24px;
            margin: 0 10px;
        }
    }
</style>