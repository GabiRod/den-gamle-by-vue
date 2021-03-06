<template>
    <div>
        <gmap-map
            :center="center"
            :zoom="19"
            :options="{ disableDefaultUI: true, gestureHandling: 'greedy' }"
            style="width:100%;"
        >
            <gmap-marker
                :position="userLocation"
                :icon="{ url: require('../assets/img/custom-icon.gif') }"
            ></gmap-marker>
            <gmap-marker
                :key="index"
                v-for="(m, index) in routeHouses"
                :position="{ lat: m.lat, lng: m.lng }"
                :description="m.description"
                :title="m.title"
                :year="m.year"
                :task="m.task"
                :id="m.id"
                ref="marker"
                @click="putData(m.description, m.title, m.id)"
            ></gmap-marker>
        </gmap-map>
    </div>
</template>

<style lang="scss">
.vue-map-container {
    z-index: -1;
    position: initial;
}
.information-pop {
    z-index: 999;
    position: fixed;
}
</style>

<script>
import { db } from '../db';

export default {
    name: 'googleMap',
    components: {},
    data() {
        return {
            houses: this.$root.houses,
            center: { lat: 56.15956, lng: 10.190563 },
            markers: [],
            places: [],
            userLocation: this.$root.userLocation,
            currentPlace: null,
            userPos: this.$root.userPos,
            popUpTriggerId: ''
        };
    },

    computed: {
        routeHouses() {
            const houseIds = this.$root.selectedRoute
                ? this.$root.selectedRoute.houses
                : [];
            const filteredHouses = this.$root.houses.filter(house =>
                houseIds.some(houseId => {
                    return houseId === house.id;
                })
            );

            return filteredHouses;
        }
    },

    mounted() {
        this.geolocate();

        //this function would prevent the app to display pieces of the map
        //outside of Den Gamle By so the user can use it only there plus
        //they can't get lost
        //this.$refs.gmap.$mapObject.fitBounds(boundaries of DGB);
    },

    methods: {
        // receives a place object via the autocomplete component
        setPlace(place) {
            this.currentPlace = place;
        },

        geolocate: function() {
            navigator.geolocation.watchPosition(position => {
                this.center = {
                    lat: position.coords.latitude,
                    lng: position.coords.longitude
                };
                this.$enableHighAccuracy = true;

                this.userLocation.lat = position.coords.latitude;
                this.userLocation.lng = position.coords.longitude;
                db.collection('userPos').add({
                    lat: position.coords.latitude,
                    lng: position.coords.longitude,
                    timestamp: Date.now()
                });

                for (let filteredHouse of this.routeHouses) {
                    if (
                        Number(position.coords.longitude).toFixed(4) ==
                            Number(filteredHouse.lng).toFixed(4) &&
                        Number(position.coords.latitude).toFixed(4) ==
                            Number(filteredHouse.lat).toFixed(4) &&
                        this.popUpTriggerId != filteredHouse.id
                    ) {
                        this.$emit('positionMatch', filteredHouse.id);
                        this.popUpTriggerId = filteredHouse.id;
                        navigator.vibrate([300, 100, 300]);
                    } else {
                        // console.log(filteredHouse.lng);
                        // console.log(this.userLocation.lng);
                    }
                }
            });
        },
        putData(description, title, id) {
            this.$emit('pin-clicked', id);
        }
    }
};
</script>
