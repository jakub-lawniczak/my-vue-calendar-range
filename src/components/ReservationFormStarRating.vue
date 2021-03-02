<template>
<div class="star-rating-hld">
  <div class="star-rating">
    <span v-for="n in max" v-bind:key="n">&star;</span>
    <p class="star-rating__reviews">{{ reviewsNumber }}</p>
    <div class="star-rating__current" :style="{width: rating + '%'}">
    <span v-for="m in max" v-bind:key="m">&starf;</span>
    </div>
  </div>
  </div>
</template> 

<script>

export default {
    props: ['max', 'stars', 'reviewsNumber'],
    computed: {
        rating: function() {
        return (this.stars / this.max) * 100
        }
    }
}
</script>

<style lang="scss" scoped>
.star-rating-hld {
    display: flex;
    align-items: center;
}
.star-rating {
    color: #3B747A;
    display: flex;
    font-size: 14px;
    position: relative;
    &__current {
        position: absolute;
        top: 0;
        overflow: hidden;
        white-space: nowrap;
    }
    &__reviews {
        position: absolute;
        top: 4px;
        left: 110%;
        font-size: 11px;
        font-weight: 700;
    }
}

</style>