<template>
  <div>
      <select v-on:change="handleChange()" v-model="film">
          <option value="" disabled="disabled">Choose film</option>
          <option v-for="film in films" :value="film" :key="film.id">{{film.title}}</option>
      </select>
  </div>
</template>

<script>
import {eventBus} from '../main.js'

export default {
    props: ['films'],
    data() {
        return {
            film: null
        }
    },
    methods: {
        handleChange: function () {
            eventBus.$emit('selected-film', this.film)
        }
    }
}
</script>

<style>

</style>