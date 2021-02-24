<template>
  <div class="form-header">
    <p class="form-header__price">
      {{ price }} zł
    </p>
    <ReservationFormStarRating :max="5" :stars="this.randomValue()" :reviewsNumber="this.randomReviewsNumber()"></ReservationFormStarRating>
  </div>
</template>

<script>
import ReservationFormStarRating from './ReservationFormStarRating.vue'
export default {
    props: ['price'],
    components: {
        ReservationFormStarRating
    },  
    methods: {
        randomValue: function () {
            return (Math.random()*4+1).toFixed(2);
        },
        randomReviewsNumber: function () {
            return Math.ceil(Math.random()*2500+1);
        }
    },
    mounted() {
        this.randomValue();
        this.randomReviewsNumber();
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
