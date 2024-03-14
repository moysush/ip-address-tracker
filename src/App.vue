<script setup>
import { onMounted, ref } from "vue";
import axios from "axios";
import L from "leaflet";

const mapContainer = ref();
const map = ref();
const marker = ref();
const apiKey = "#"; //at_8UMPaDeiv6X8oz4pn9lTo7AZNiyHU
const ipAddress = ref();
const location = ref();
const timezone = ref();
const isp = ref();
const geolocationInfo = ref(null);
const inputData = ref();

// function search() {
//   if (typeof inputData == "number") {
//     ipAddress.value = inputData.value;
//   } else {
//     // location.value = `${response.data.location.region}, ${response.data.location.country}, ${response.data.location.geonameId}`;
//   }
// }

onMounted(async () => {
  //api
  // const response = await axios.get(
  //   `https://geo.ipify.org/api/v2/country,city?apiKey=${apiKey}`
  // );
  // ipAddress.value = response.data.ip;
  // location.value = `${response.data.location.region}, ${response.data.location.country}, ${response.data.location.geonameId}`;
  // timezone.value = response.data.location.timezone;
  // isp.value = response.data.isp;
  // geolocationInfo.value = response.data;
  // const { lat, lng } = response.data.location;

  //map
  map.value = L.map(mapContainer.value).setView([23.78822, 90.37359], 13);
  marker.value = L.marker([23.78822, 90.37359]).addTo(map.value);
    //layout
  L.tileLayer("https://tile.openstreetmap.org/{z}/{x}/{y}.png", {
    maxZoom: 19,
    attribution:
      '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>',
  }).addTo(map.value);

  console.log(geolocationInfo.value);
});
</script>

<template>
  <header></header>
  <body>
    <div class="top-cover"></div>
    <div ref="mapContainer" class="map">{{}}</div>
    <div class="container">
      <main>
        <h1>IP Address Tracker</h1>
        <div class="input-group mb-3">
          <input
            :v-model="inputData"
            type="text"
            placeholder="Search for any IP address or domain"
            class="form-control"
            aria-label="search-box"
            aria-describedby="button-addon2"
          />
          <button class="btn btn-dark" type="button">
            <img src="../images/icon-arrow.svg" alt="icon-arrow" />
          </button>
        </div>
        <a href="#" @click="search()"> <i></i> </a>
        <div class="result card-group">
          <div class="card card-body text-bg-light mb-3 p-4" style="max-width: 18rem">
              <h5 class="card-title">IP Address</h5>
              <p class="card-text">
                {{ ipAddress }}
              </p>
          </div>
          <div class="card card-body text-bg-light mb-3 p-4" style="max-width: 18rem">
              <h5 class="card-title">Location</h5>
              <p class="card-text">
                {{ location }}
              </p>
          </div>
          <div class="card card-body text-bg-light mb-3 p-4" style="max-width: 18rem">
              <h5 class="card-title">Timezone</h5>
              <p class="card-text">
                {{ timezone }}
              </p>
          </div>
          <div class="card card-body text-bg-light mb-3 p-4" style="max-width: 18rem">
              <h5 class="card-title">ISP</h5>
              <p class="card-text">
                {{ isp }}
              </p>
          </div>
          UTC
        </div>
      </main>
      
      <footer class="attribution">
        Challenge by
        <a href="https://www.frontendmentor.io?ref=challenge" target="_blank"
        >Frontend Mentor</a
        >. Coded by <a href="#">Your Name Here</a>.
      </footer>
    </div>
  </body>
</template>

<style scoped>
@media (min-width: 1024px) {
}

.form-control {
  cursor: pointer;
}
.top-cover {
  position: fixed;
  top: 0;
  left: 0;
  background-image: url("../images/pattern-bg-desktop.png");
  background-size: contain;
  background-repeat: no-repeat;
  width: 100vw;
  height: 100%;
  /* z-index: 1; */
}
.map{
  position: fixed;
  top: calc(100% - 80%);
  left: 0;
  width: 100vw;
  height: 100%;
}
</style>
