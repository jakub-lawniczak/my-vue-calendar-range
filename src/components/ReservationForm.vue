<template>
  <div class="form-box">
    <div class="form-box__content">
      <ReservationFormHeader :price="price"></ReservationFormHeader>
      <ReservationFormDatePicker></ReservationFormDatePicker>
    </div>
  </div>
</template>

<script>
import ReservationFormHeader from './ReservationFormHeader.vue'
import ReservationFormDatePicker from './ReservationFormDatePicker.vue'
export default {
  props: ['price'],
  name: 'ReservationForm',
  components: {
    ReservationFormHeader,
    ReservationFormDatePicker
  },

}
</script>

<style scoped lang="scss">
.form-box {
  width: 440px;
  height: 580px;
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
