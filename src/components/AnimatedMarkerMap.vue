<template>
  <div>
    <div class="header-margins">
      Google Maps Animated SVG Marker in Vue.js by
      <a target="_blank" href="nightprogrammer.com">nightprogrammer.com</a>
    </div>
    <div id="animated-marker-map" class="map-margins"></div>
  </div>
</template>

<script>
import loadGoogleMapsApi from "load-google-maps-api";
import { gMapsApiKey } from "./../constans";

export default {
  name: "AnimatedMarkerMap",
  data() {
    return {
      map: null,
    };
  },
  mounted() {
    loadGoogleMapsApi({
      key: gMapsApiKey,
      libraries: ["places", "drawing", "geometry"],
    }).then(async () => {
      const mapZoom = 12;
      const { google } = window;
      const mapOptions = {
        zoom: mapZoom,
        mapTypeId: google.maps.MapTypeId.HYBRID,
        center: new google.maps.LatLng({ lat: 23, lng: 57 }),
        mapTypeControl: true,
        streetViewControl: false,
        mapTypeControlOptions: {
          position: google.maps.ControlPosition.BOTTOM_LEFT,
        },
      };
      this.map = new google.maps.Map(
        document.getElementById("animated-marker-map"),
        mapOptions
      );

      const newIcon = {
        url: "http://svgur.com/i/d9y.svg",
        anchor: new google.maps.Point(25, 50),
        scaledSize: new google.maps.Size(50, 50),
      };

      const marker = new google.maps.Marker({
        position: this.map.getCenter(),
        map: this.map,
        icon: newIcon,
      });

      this.map.addListener("click", (mapsMouseEvent) => {
        marker.setPosition(mapsMouseEvent.latLng);
      });
    });
  },
};
</script>

<style scoped>
.header-margins {
  margin-left: 40px;
  margin-top: 20px;
}

.map-margins {
  height: 400px;
  width: 600px;
  margin: 30px 40px;
}
</style>