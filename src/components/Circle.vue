<template>
    <div class="container"></div>
</template>
<script>
export default {
  props: {
    city: Object,
    google: Object,
    map: Object,
  },
  mounted() {
     new this.google.maps.Circle({
      strokeColor: "#FF0000",
      strokeOpacity: 0.8,
      strokeWeight: 2,
      fillColor: "#FF0000",
      fillOpacity: 0.35,
      map: this.map,
      center: this.city.center,
      radius: Math.sqrt(this.city.population) * 100,
    });
    this.map.setZoom(6)
    this.map.setCenter(this.city.center)
  },
};
</script>

<style>
</style>