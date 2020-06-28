<template>
<div>
    <ul v-if="selectedFilm">
        <h3>{{selectedFilm.title}} - Film description</h3>
        <li>{{selectedFilm.description}}</li>
        <div v-if="peopleInFilm.length > 0" id="people-in-film">
            <h3>People in this film:</h3>
            <li v-for="person in peopleInFilm" :key="person.id">{{person.name}}</li>
        </div>
        <div v-else>
            <h3>People in this film: (there's no one registered)</h3>
        </div>
        <div v-if="locationsInFilm.length > 0" id="locations-in-film">
            <h3>Locations in this film:</h3>
            <li v-for="location in locationsInFilm" :key="location.id">{{location.name}}</li>
        </div>
        <div v-else>
            <h3>Locations in this film: (there are no locations registered)</h3>
        </div>
        <div v-if="vehiclesInFilm.length > 0" id="vehicles-in-film">
            <h3>Vehicles in this film:</h3>
            <li v-for="vehicle in vehiclesInFilm" :key="vehicle.id">{{vehicle.name}}</li>
        </div>
        <div v-else>
            <h3>Vehicles in this film: (there are no vehicles registered)</h3>
        </div>
    </ul>
</div>
</template>

<script>

export default {
    props: ['people', 'locations', 'vehicles', 'selectedFilm'],
    computed: {
        peopleInFilm: function () {
            let peopleFound = [];
            let peopleIdFound = [];
            let peoplesFilms = {};
            this.people.forEach((person) => {
                peoplesFilms[`${person.id}`] = person.films;
            })
            for (let personId in peoplesFilms) {
                peoplesFilms[personId].forEach((film) => {
                    const filmPathname = new URL(film).pathname;
                    const pathnameArray = filmPathname.split('/');
                    const filmID = pathnameArray[pathnameArray.length - 1];
                    if (filmID === this.selectedFilm.id) {
                        peopleIdFound.push(personId);
                    }
                })
            }
            return peopleFound = this.people.filter((person) => {
                return peopleIdFound.includes(person.id);
            })
        },

        locationsInFilm: function () {
            let locationsFound = [];
            let locationsIdFound = [];
            let filmLocations = {};
            this.locations.forEach((location) => {
                filmLocations[`${location.id}`] = location.films;
            })
            for (let locationId in filmLocations) {
                filmLocations[locationId].forEach((film) => {
                    const filmPathname = new URL(film).pathname;
                    const pathnameArray = filmPathname.split('/');
                    const filmID = pathnameArray[pathnameArray.length - 1];
                    if (filmID === this.selectedFilm.id) {
                        locationsIdFound.push(locationId);
                    }
                })
            }
            return locationsFound = this.locations.filter((location) => {
                return locationsIdFound.includes(location.id);
            })
        },

        vehiclesInFilm: function () {
            let vehiclesFound = [];
            let vehiclesIdFound = [];
            let vehiclesInFilm = {};
            this.vehicles.forEach((vehicle) => {
                const filmPathname = new URL(vehicle.films).pathname;
                const pathnameArray = filmPathname.split('/');
                const filmID = pathnameArray[pathnameArray.length - 1];
                if (filmID === this.selectedFilm.id) {
                    vehiclesIdFound.push(vehicle.id);
                }
            })
            return vehiclesFound = this.vehicles.filter((vehicle) => {
                return vehiclesIdFound.includes(vehicle.id);
            })
        }
    }
}
</script>

<style>

</style>