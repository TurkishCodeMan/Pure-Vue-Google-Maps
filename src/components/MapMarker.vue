<template>
  <div class="container"></div>
</template>
<script>
export default {
  props: {
    google: { type: Object, required: true },
    map: { type: Object, required: true },
    marker: { type: Object, required: true },
  },
  mounted() {
    let marker = new this.google.maps.Marker({
      position: this.marker.position,
      marker: this.marker,
      map: this.map,
      draggable: true,
      animation: this.google.maps.Animation.DROP,
    });

    //Click Marker
    this.google.maps.event.addListener(marker, "click", () => {
      let info = new this.google.maps.InfoWindow({
        content: this.marker.info,
      });
      info.open(this.map, marker);
    });
  },
};
</script>

<style>
</style>