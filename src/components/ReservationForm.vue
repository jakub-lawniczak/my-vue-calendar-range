<template>
  <div class="form-box">
    <div class="form-box__content">
      <ReservationFormHeader :price="price" :max="max" :stars="stars" :reviewsNumber="reviewsNumber"></ReservationFormHeader>
      <ReservationFormDatePicker :disabledDays="disabledDays" :dateRange="dateRange" @update:dateRange="$emit('update:dateRange', $event)"></ReservationFormDatePicker>
      <ReservationFormData :value='reservationData' @input="$emit('update:reservationData', $event)"></ReservationFormData> 
    </div>
  </div>
</template>

<script>
import ReservationFormHeader from './ReservationFormHeader.vue'
import ReservationFormDatePicker from './ReservationFormDatePicker.vue'
import ReservationFormData from './ReservationFormData.vue'
export default {
  props: ['price', 'max', 'stars', 'reviewsNumber', 'disabledDays', 'dateRange', 'reservationData'],
  name: 'ReservationForm',
  components: {
    ReservationFormHeader,
    ReservationFormDatePicker,
    ReservationFormData
  },

}
</script>
<style scoped lang="scss">
.form-box {
  width: 440px;
  background-color: #ECECEC;
  position: relative;
  margin: 22px auto;
  padding: 10px 10px 70px;
  z-index: 1;
  &:before {
    content: '';
    position: absolute;
    top: 10px;
    right: 10px;
    bottom: 70px;
    left: 10px;
    background-color: transparent;
    border: 1px solid #D9D9D9;
    z-index: -1;
  }
  &__content {
    padding: 0 28px;
  }
}
</style>
