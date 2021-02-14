<template>
  <div class="container-fluid">
    <div class="container">
      <div class="form-group">
        <input type="text" class="form-control" v-model="address" />
        <button class="btn btn-outline-danger" @click.prevent="geocodeAddress">
          Geocode
        </button>
      </div>
    </div>
    <div class="google-map mt-3" ref="googleMap"></div>
    <template v-if="Boolean(this.google) && Boolean(this.map)">
      <slot :google="google" :map="map" />

      <GoogleMarker
        v-for="marker in markers"
        :key="marker.id"
        :google="google"
        :map="map"
        :marker="marker"
      >
      </GoogleMarker>

      <GoogleCircle
        v-for="city in cities"
        :key="city.id"
        :google="google"
        :map="map"
        :city="city"
      >
      </GoogleCircle>
    </template>
  </div>
</template>

<script>
import GoogleMapsApiLoader from "google-maps-api-loader";
import GoogleMarker from "./MapMarker";
import GoogleCircle from "./Circle";
export default {
  data() {
    return {
      apiKey: "YOUR_KEY",
      google: null,
      map: null,
      geocoder: null,
      address: "Suriye",
      id: 1,
      markers: [
        { id: "a", position: { lat: 35, lng: 38 }, info: "Güzell" },
        { id: "b", position: { lat: 40, lng: 37 }, info: "Güzell Hoss" },
        { id: "c", position: { lat: 6, lng: 97 }, info: "Güzell" },
      ],
      cities: [
        { id: "1", center: { lat: 41.878, lng: -87.629 }, population: 2714856 },
        { id: "2", center: { lat: 40.714, lng: -74.005 }, population: 8405837 },
        { id: "3", center: { lat: 49.25, lng: -123.1 }, population: 603502 },
      ],
    };
  },
  components: { GoogleMarker, GoogleCircle },

  //DOM render once
  computed: {
    mapConfig: {
      get: function () {
        return {
          zoom: 10,
          center: { lat: 35, lng: 38 },
          mapTypeId: "hybrid",
          mapTypeControlOptions: {
            mapTypeIds: ["terrain", "roadmap", "hybrid"],
            style: this.google.maps.MapTypeControlStyle.DROPDOWN_MENU,
          },
        };
      },
      set: function (value) {
        console.log(value);
        this.mapConfig = value;
      },
    },
  },
  methods: {
    initializeMap() {
      const mapContainer = this.$refs.googleMap;
      this.map = new this.google.maps.Map(mapContainer, this.mapConfig);
      this.geocoder = new this.google.maps.Geocoder();
    },
    geocodeAddress() {
      if (this.address != "") {
        this.geocoder.geocode({ address: this.address }, (result, status) => {
          if (status == "OK") {
            this.markers.push({
              id: this.id,
              position: result[0].geometry.location,
            });
            this.id++;
            this.map.setCenter(result[0].geometry.location);
            this.map.setZoom(12);
          }
        });
      }
    },
  },

  async mounted() {
    const googleMapApi = await GoogleMapsApiLoader({
      apiKey: this.apiKey,
    });
    this.google = googleMapApi;
    this.initializeMap();
  },
};
</script>

<style scoped>
.google-map {
  width: 100%;
  height: 500px;
}
</style>