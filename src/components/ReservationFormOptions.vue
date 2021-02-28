<template>
    <div class="form-options">
        <select>
            <option v-for="(person, key) in persons" v-bind:key="key">
                {{person}}
            </option>
        </select>
    </div>
</template>

<script>
export default {
    data() {
        return {
            persons: [1,2,3,4,5]
        }
    }

}
</script>

<style>

</style>