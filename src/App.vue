

<template>
  <div id="main">
    <mgl-map
      :center="[-75.163471, 39.953338]"
      :zoom="12"
    >

      <mgl-geolocation-control/>

      <mgl-raster-source
        source-id="test"
        :tiles="stormwater"
      >
      <!-- :tiles="tilesZoning" -->
      <!-- :tile-size="256" -->
      
      <!-- type="'raster'" -->
      <!-- :source="rasterSource" -->
      <!-- :layer="rasterLayer" -->
      <!-- source="test" -->

        <mgl-raster-layer
          layer-id="test-wawa"
          :minzoom="0"
          :maxzoom="22"
        />
      
      
      </mgl-raster-source>

    </mgl-map>
  </div>
</template>

<script>

import { MglDefaults } from 'vue-maplibre-gl';


MglDefaults.style = 'https://api.maptiler.com/maps/streets/style.json?key=cQX2iET1gmOW38bedbUh';
console.log('MglDefaults', MglDefaults);

// import VueMaplibreGl from 'vue-maplibre-gl'
// app.use(VueMaplibreGl)
import { MglMap } from 'vue-maplibre-gl'
import { MglGeolocationControl } from 'vue-maplibre-gl';
import { MglRasterSource } from 'vue-maplibre-gl'
import { MglRasterLayer } from 'vue-maplibre-gl'
// app.component('MglMap', MglMap)

export default {
  name: 'App',
  components: {
    MglMap,
    MglGeolocationControl,
    MglRasterSource,
    MglRasterLayer,
  },
  data() {
    return {
      rasterSource: {
        'type': 'raster',
        'tiles': ['https://tiles.stadiamaps.com/tiles/stamen_watercolor/{z}/{x}/{y}.jpg'],
        'tileSize': 256,
      },
      rasterLayer: {
        id: 'pwd',
        type: 'raster',
      },
      tilesCity: ['https://tiles.arcgis.com/tiles/fLeGjb7u4uXqeF9q/arcgis/rest/services/CityImagery_2022_2in/MapServer/tile/{z}/{y}/{x}'],
      // tilesZoning: ['https://citygeo-geocoder-pub.databridge.phila.gov/arcgis/rest/services/Atlas/ZoningMap/MapServer/tile/{z}/{y}/{x}'],
      tilesZoning: ['https://citygeo-geocoder-pub.databridge.phila.gov/arcgis/rest/services/Atlas/ZoningMap/MapServer/export?dpi=120\
        &transparent=true\
        &format=png32\
        &bbox={bbox-epsg-3857}\
        &bboxSR=3857\
        &imageSR=3857\
        &size=2300,2300\
        &f=image\
      '],
      tilesZoning2: ['https://citygeo-geocoder-pub.databridge.phila.gov/arcgis/rest/services/Atlas/ZoningMap/MapServer/tile/{z}/{y}/{x}'],
      stormwater: ['https://stormwater.phila.gov/arcgis/rest/services/parcel_viewer/pv_data/MapServer/export?dpi=110\
        &transparent=true\
        &format=png36\
        &bbox={bbox-epsg-3857}\
        &bboxSR=3857\
        &imageSR=3857\
        &size=512,512\
        &f=image\
      '],
      pwdParcels: ['https://tiles.arcgis.com/tiles/fLeGjb7u4uXqeF9q/arcgis/rest/services/PWDParcel_ImageryOverlay/MapServer/export?dpi=110\
        &transparent=true\
        &format=png36\
        &bbox={bbox-epsg-3857}\
        &bboxSR=3857\
        &imageSR=3857\
        &size=512,512\
        &f=image\
      '],
    }
  },
  mounted() {
    console.log('mounted')
  },
}

</script>

<style>

@import "maplibre-gl/dist/maplibre-gl.css";
@import "vue-maplibre-gl/dist/vue-maplibre-gl.css";

#main {
  height: 80vh;
  width: 80vw;
}

</style>
