<template>
  <div id="viewDiv" class="balt-theme"></div>
</template>
<script>
  /* eslint-disable */
  import { loadModules } from 'esri-loader';
  export default {
    mounted() {
      this.loadMap();
    },
    methods: {
      loadMap() {
        loadModules([ 'esri/Map', 
                      'esri/views/MapView',
                      'esri/WebMap',
                      'esri/layers/Layer',
                      'esri/layers/FeatureLayer',
                      'esri/geometry/Geometry',
                      'esri/widgets/LayerList',
                      'esri/widgets/BasemapToggle',
                      'esri/widgets/BasemapGallery'
                      ], {
            css: true
          })
          .then(([ArcGISMap, MapView, WebMap, Layer, FeatureLayer, Geometry, LayerList,
                  BasemapToggle, BasemapGallery]) => {
            // create map with the given options
            const map = new ArcGISMap({
              basemap: 'topo-vector'
            });

            var features = [
                            {
                              geometry: {
                                type: "point",
                                x: 118.015776, 
                                y: -2.6000285
                              },
                              attributes: {
                                ObjectID: 1,
                                DepArpt: "KATL",
                                MsgTime: Date.now(),
                                FltId: "UAL1",
                                Title:"Example Layer"
                              }
                            }
                          ];
            /*
            var features = [
                            {
                              attributes: {
                                FID: 4,
                                Id: 0,
                                STATUS_SWH: "Cadangan LP2B",
                                HA: 0.866355465761
                              },
                              geometry: {
                                type:"Polygon",
                                spatialReference: {
                                  wkid: 102100,
                                  latestWkid: 3857
                                },
                                rings: [
                                  [
                                    [12276040, -822544],
                                    [12276043, -822598],
                                    [12276013, -822660],
                                    [12275971, -822669],
                                    [12275954, -822608],
                                    [12275970, -822556],
                                    [12275984, -822543],
                                    [12276006, -822556],
                                    [12276040, -822528],
                                    [12276040, -822544]
                                  ]
                                ]
                              }
                            }
                          ]; 
            */
            
            var layer = new FeatureLayer({
              source: features,  // autocast as a Collection of new Graphic()
              objectIdField: "ObjectID",
              title:"Judul Halaman"
            });          
            map.add(layer,0);

            const fl = new FeatureLayer({
              url: "https://portal.ina-sdi.or.id/gis/rest/services/IGT/Atlas_250K_PerkembanganWilayahAdministrasi/MapServer/25?f=pjson"
            });
            map.add(fl);  // adds the layer to the map
            

            const sw = new FeatureLayer({
              url: "https://portal.ina-sdi.or.id/gis/rest/services/IGT/Sawah_Jawa/MapServer?f=pjson"
            });
            map.add(sw);  // adds the layer to the map
          
            // assign map to this view
            this.view = new MapView({
              container: this.$el,
              map: map,
              center: [118.015776, -2.6000285], // longitude, latitude center of Indonesia
              zoom: 5
            });

            //Layer List UI
            var layerList = new LayerList({
              view: this.view
            });
            // Adds widget below other elements in the top left corner of the view
            this.view.ui.add(layerList, {
              position: "top-right"
            });

            //Basemap List
            var basemapToggle = new BasemapToggle({
              view: this.view,
              nextBasemap: "satellite"
            });

            this.view.ui.add(basemapToggle, "bottom-right");

            //Basemap List
            /*
            var basemapGallery = new BasemapGallery({
              view: this.view,
              source: {
                portal: {
                  url: "https://www.arcgis.com",
                  useVectorBasemaps: true // Load vector tile basemaps
                }
              }
            });

            this.view.ui.add(basemapGallery, "bottom-right");
            */
          });
      }
    }
  }
</script>
<style>
  /* esri styles */
  @import url('https://js.arcgis.com/4.4/esri/themes/light/main.css');

  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
  }

  #nav {
    padding: 30px;
  }

  #nav a {
    font-weight: bold;
    color: #2c3e50;
  }

  #nav a.router-link-exact-active {
    color: #42b983;
  }
  #viewDiv {
    position: absolute;
    top: 3.5rem;
    bottom: 0;
    left: 0;
    right: 0;
  }
</style>