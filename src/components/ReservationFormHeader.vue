<template>
  <div class="form-header">
    <p class="form-header__price">
      {{ price }} zł
    </p>
    <ReservationFormStarRating :max="max" :stars="stars" :reviewsNumber="reviewsNumber"></ReservationFormStarRating>
  </div>
</template>

<script>
import ReservationFormStarRating from './ReservationFormStarRating.vue'
export default {
    props: ['price', 'max', 'stars', 'reviewsNumber'],
    components: {
        ReservationFormStarRating
    },
  }
</script>
<style scoped lang="scss">
.form-header {
    padding: 28px 0;
    border-bottom: 1px solid #D9D9D9;
    &__price {
        font-size: 24px;
        font-weight: 700;
    }
}
</style>
