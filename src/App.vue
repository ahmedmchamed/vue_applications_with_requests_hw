<template>
  <div id="app">
    <p>Select a film below</p>
    <list-films :films="films"></list-films>
    <list-film-details :people="people" :locations="locations" :vehicles="vehicles" :selectedFilm="selectedFilm"></list-film-details>
  </div>
</template>

<script>
import ListFilms from './components/ListFilms.vue';
import ListFilmDetails from './components/ListFilmDetails.vue';
import {eventBus} from './main.js';

export default {
  name: 'App',
  data() {
    return {
      films: [],
      selectedFilm: null,
      people: {},
      locations: {},
      vehicles: {} 
    }
  },
  mounted() {
    this.studioGhibliFilms();
    this.studioGhibliPeople();
    this.studioGhibliLocations();
    this.studioGhibliVehicles();

    eventBus.$on('selected-film', (film) => {
      this.selectedFilm = film
    })
  },
  methods: {
    studioGhibliFilms: function () {
      fetch('https://ghibliapi.herokuapp.com/films')
      .then(response => response.json())
      .then((allFilms) => {
        this.films = allFilms;
      })
    },
    studioGhibliPeople: function () {
      fetch('https://ghibliapi.herokuapp.com/people')
      .then(response => response.json())
      .then((allPeople) => {
        this.people = allPeople;
      })
    },
    studioGhibliLocations: function () {
      fetch('https://ghibliapi.herokuapp.com/locations')
      .then(response => response.json())
      .then((allLocations) => {
        this.locations = allLocations;
      })
    },
    studioGhibliVehicles: function () {
      fetch('https://ghibliapi.herokuapp.com/vehicles')
      .then(response => response.json())
      .then((allVehicles) => {
        this.vehicles = allVehicles;
      })
    }
  },
  components: {
    'list-films': ListFilms,
    'list-film-details': ListFilmDetails
  }
}
</script>

<style>

</style>
