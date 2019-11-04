<template>
  <div></div>
</template>

<script>
import { loadModules } from 'esri-loader';

export default {
  name: 'web-map',
  props: {

    basemap: {
      required: false,
      type: String,
      default: 'topo-vector'
    },
    center: {
      required: true,
      type: Array,
      default: [-118, 34]
    },
    zoom: {
      required: true,
      type: Number,
      default: 8
    }

  },
  mounted() {
    // lazy load the required ArcGIS API for JavaScript modules and CSS
    loadModules(['esri/Map', 'esri/views/MapView'], { css: true })
    .then(([ArcGISMap, MapView]) => {
      const map = new ArcGISMap({
        basemap: this.basemap
      });

      this.view = new MapView({
        container: this.$el,
        map: map,
        center: this.$props.center,
        zoom: this.zoom
      });
    });
  },
  beforeDestroy() {
    if (this.view) {
      // destroy the map view
      this.view.container = null;
    }
  }
};

</script>

<style scoped>
div {
  padding: 0;
  margin: 0;
  width: 100%;
  height: 100%;
}
</style>