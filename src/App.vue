<template>
  <body>
    <div id="app">
      <Map></Map>
    </div>
  </body>
</template>

<script>
import Map from "./components/Map.vue";

export default {
  name: "App",
  data() {
    return {
  
    };
  },
  components: {
    Map,
  },
  mounted() {
    // //Add Geoccoder || Marker
    // EventBus.$on("result", (result) => {
    //   this.markers.push({
    //     id:this.id,
    //     position:result[0].geometry.location,
    //   })
    //   this.id++;
    // });
  },
};
</script>

<style>
body {
  margin: 0;
  padding: 0;
}
</style>
