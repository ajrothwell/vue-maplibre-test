

<template>
  <div id="main">
    <mgl-map
      :center="[-75.163471, 39.953338]"
      :mapStyle="currentStyle"
      :zoom="12"
      @map:click="mapClicked"
    >

      <mgl-navigation-control position="bottom-left"/>
      <mgl-geolocation-control position="bottom-left"/>

      <mgl-custom-control position="top-left" :no-classes="!useClasses">
        <mapbox-address-input />
      </mgl-custom-control>

      <!-- <mgl-custom-control v-if="showCustomControl" position="top-left" :no-classes="!useClasses"> -->
      <mgl-custom-control position="top-right" :no-classes="!useClasses">
        <mgl-button
          :type="'text'"
          @click="buttonClicked"
        >
          <!-- <FontAwesomeIcon icon="layer-group" size="lg" /> -->
          <img
            :src="mapButton"
            style="max-width: 100%; border-radius: 3px"
          />
          <!-- <img src="./assets/imagery_small.png"/> -->
        </mgl-button>
      </mgl-custom-control>

      <!-- :tiles="stormwater" -->
      <!-- :tiles="tilesCity" -->
      <!-- :tiles="tilesZoning" -->
      <!-- <mgl-raster-source
        source-id="city-imagery"
        :tiles="stormwater"
        :tile-size="256"
      >
        <mgl-raster-layer
          layer-id="city-imagery"
          :minzoom="0"
          :maxzoom="22"
        />
      </mgl-raster-source> -->

      <!-- <mgl-raster-source
        source-id="city-imagery-labels"
        :tiles="tilesZoning"
        :tile-size="256"
      >
        <mgl-raster-layer
          layer-id="city-imagery-labels"
          :minzoom="0"
          :maxzoom="22"
        />
      </mgl-raster-source> -->

      <mgl-marker
        :coordinates="markerCoordinates"
        color="#cc0000"
        :scale=".75"
      />

    </mgl-map>
  </div>
</template>

<script>

// import { MglDefaults } from 'vue-maplibre-gl';


// MglDefaults.style = 'https://api.maptiler.com/maps/streets/style.json?key=cQX2iET1gmOW38bedbUh';
// console.log('MglDefaults', MglDefaults);

import { MglMap } from 'vue-maplibre-gl'
import { MglGeolocationControl } from 'vue-maplibre-gl';
import { MglRasterSource } from 'vue-maplibre-gl'
import { MglRasterLayer } from 'vue-maplibre-gl'
import { MglMarker } from 'vue-maplibre-gl'
import { MglNavigationControl } from 'vue-maplibre-gl'
import { MglCustomControl } from 'vue-maplibre-gl'
import { MglButton } from 'vue-maplibre-gl'

import MapboxAddressInput from './components/MapboxAddressInput.vue';

// let checkmarkPath = './assets/check_@2X.png';
// import { mdiCursorDefaultClick } from '@mdi/js';

import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import { library } from '@fortawesome/fontawesome-svg-core';
import { faCube, faLayerGroup } from '@fortawesome/free-solid-svg-icons'
// console.log('faCube', faCube, 'mdiCursorDefaultClick:', mdiCursorDefaultClick);
library.add(faCube, faLayerGroup);


export default {
  name: 'App',
  setup() {
    console.log('setup');
    // library.add(faCube);
    // const map = useMap()
    // return { map }
  },
  components: {
    MglMap,
    MglGeolocationControl,
    MglRasterSource,
    MglRasterLayer,
    MglMarker,
    MglNavigationControl,
    MglCustomControl,
    MglButton,
    FontAwesomeIcon,
    MapboxAddressInput,
  },
  data() {
    return {
      currentStyle: null,
      mapButton: "./src/assets/imagery_small.png",
      mapStyleTest: {
        version: 8,
        sources: {
          pwd: {
            tiles: [
              'https://tiles.arcgis.com/tiles/fLeGjb7u4uXqeF9q/arcgis/rest/services/CityBasemap/MapServer/tile/{z}/{y}/{x}',
            ],
            type: 'raster',
            tileSize: 256,
          },
          pwdLabels: {
            tiles: [
              'https://tiles.arcgis.com/tiles/fLeGjb7u4uXqeF9q/arcgis/rest/services/CityBasemap_Labels/MapServer/tile/{z}/{y}/{x}',
            ],
            type: 'raster',
            tileSize: 256,
          },
        },
        layers: [
          {
            id: 'pwd',
            source: 'pwd',
            type: 'raster',
          },
          {
            id: 'pwdLabels',
            source: 'pwdLabels',
            type: 'raster',
          },
        ],
      },
      mapStyleTest2: {
        version: 8,
        sources: {
          imagery: {
            tiles: [
              'https://tiles.arcgis.com/tiles/fLeGjb7u4uXqeF9q/arcgis/rest/services/CityImagery_2022_2in/MapServer/tile/{z}/{y}/{x}',
            ],
            type: 'raster',
            tileSize: 256,
          },
          imageryLabels: {
            tiles: [
            'https://tiles.arcgis.com/tiles/fLeGjb7u4uXqeF9q/arcgis/rest/services/CityImagery_Labels/MapServer/tile/{z}/{y}/{x}',
            ],
            type: 'raster',
            tileSize: 256,
          },
          stormwater: {
            // 'https://citygeo-geocoder-pub.databridge.phila.gov/arcgis/rest/services/Atlas/ZoningMap/MapServer/export?dpi=120\
              //   &transparent=true\
              //   &format=png32\
              //   &bbox={bbox-epsg-3857}\
              //   &bboxSR=3857\
              //   &imageSR=3857\
              //   &size=512,512\
              //   &f=image\
              // ',
            tiles: [
              'https://stormwater.phila.gov/arcgis/rest/services/parcel_viewer/pv_data/MapServer/export?dpi=110\
                &transparent=true\
                &format=png36\
                &bbox={bbox-epsg-3857}\
                &bboxSR=3857\
                &imageSR=3857\
                &size=512,512\
                &f=image\
              ',
            ],
            type: 'raster',
            tileSize: 256,
          }
        },
        layers: [
          {
            id: 'imagery',
            source: 'imagery',
            type: 'raster',
          },
          {
            id: 'imageryLabels',
            source: 'imageryLabels',
            type: 'raster',
          },
          // {
          //   id: 'stormwater',
          //   source: 'stormwater',
          //   type: 'raster',
          // },
        ],
      },
      useClasses: true,
      // faCube: faCube,
      // buttonIcon: mdiCursorDefaultClick,
      buttonIcon: faCube.icon[4],
      markerCoordinates: [-75.163471, 39.953338],
      rasterSource: {
        'type': 'raster',
        'tiles': ['https://tiles.stadiamaps.com/tiles/stamen_watercolor/{z}/{x}/{y}.jpg'],
        'tileSize': 256,
      },
      rasterLayer: {
        id: 'pwd',
        type: 'raster',
      },
      basemap: null,
      labels: null,

      cityImagery: ['https://tiles.arcgis.com/tiles/fLeGjb7u4uXqeF9q/arcgis/rest/services/CityImagery_2022_2in/MapServer/tile/{z}/{y}/{x}'],
      cityImageryLabels: ['https://tiles.arcgis.com/tiles/fLeGjb7u4uXqeF9q/arcgis/rest/services/CityImagery_Labels/MapServer/tile/{z}/{y}/{x}'],
      pwd: ['https://tiles.arcgis.com/tiles/fLeGjb7u4uXqeF9q/arcgis/rest/services/CityBasemap/MapServer/tile/{z}/{y}/{x}',],
      pwdLabels: ['https://tiles.arcgis.com/tiles/fLeGjb7u4uXqeF9q/arcgis/rest/services/CityBasemap_Labels/MapServer/tile/{z}/{y}/{x}'],

      
      tilesZoning: ['https://citygeo-geocoder-pub.databridge.phila.gov/arcgis/rest/services/Atlas/ZoningMap/MapServer/export?dpi=120\
        &transparent=true\
        &format=png32\
        &bbox={bbox-epsg-3857}\
        &bboxSR=3857\
        &imageSR=3857\
        &size=512,512\
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
      pwdParcels2: ['https://tiles.arcgis.com/tiles/fLeGjb7u4uXqeF9q/arcgis/rest/services/PWDParcel_ImageryOverlay/MapServer/tile/{z}/{y}/{x}'],
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
    console.log('mounted');
    this.currentStyle = this.mapStyleTest;
    this.basemap = this.pwd;
    this.labels = this.pwdLabels;
  },
  methods: {
    buttonClicked() {
      console.log('buttonClicked');
      this.currentStyle = this.currentStyle === this.mapStyleTest ? this.mapStyleTest2 : this.mapStyleTest;
      this.mapButton = this.currentStyle === this.mapStyleTest ? "./src/assets/imagery_small.png" : "./src/assets/basemap_small.png";
      // if (this.basemap === this.pwd) {
      //   this.basemap = this.cityImagery;
      //   this.labels = this.cityImageryLabels;
      // } else {
      //   this.basemap = this.pwd;
      //   this.labels = this.pwdLabels;
      // }
    },
    mapClicked(e) {
      console.log('mapClicked', e.event.lngLat);
      // console.log('mapClicked', e, e.lngLat);
      // this.markerCoordinates = [e.lngLat.lng, e.lngLat.lat];
    },
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
