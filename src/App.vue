<script setup>
import { onMounted, ref } from "vue";
import axios from "axios";
import L from "leaflet";
import key from "./apiKey.json";

// map and apiKey
const mapContainer = ref("");
const map = ref("");
const marker = ref("");
const apiKey = key.theKey;
// infos to get
const ipAddress = ref("1.1.1.1");
const location = ref("Research, AU, 2151718");
const timezone = ref("+11:00");
const isp = ref("Cloudflare, Inc.");
// inputData
const inputData = ref("");
// lat & lng
const lat = ref("-37.7");
const lng = ref("145.18333");

// Draws the map on the screen
function mapDraw() {
  // map
  map.value = L.map(mapContainer.value, { zoomControl: false }).setView(
    [lat.value, lng.value],
    13
  );
  // marker icon
  var customIcon = L.icon({
    iconUrl: "./icon-location.svg",
    iconSize: [30, 40],
    iconAnchor: [15, 40],
  });

  L.Marker.prototype.options.icon = customIcon;
  // marker
  marker.value = L.marker([lat.value, lng.value]).addTo(map.value);
  // layout
  L.tileLayer("https://tile.openstreetmap.org/{z}/{x}/{y}.png", {
    maxZoom: 19,
    attribution:
      '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>',
  }).addTo(map.value);
}

onMounted(async () => {
  mapDraw();
  // api
  const response = await axios.get(
    `https://geo.ipify.org/api/v2/country,city?apiKey=${apiKey}`
  );
  ipAddress.value = response.data.ip;
  location.value = `${response.data.location.city}, ${response.data.location.country} ${response.data.location.geonameId}`;
  timezone.value = response.data.location.timezone;
  isp.value = response.data.isp;
  lat.value = response.data.location.lat;
  lng.value = response.data.location.lng;
  map.value.remove();
  mapDraw();
  console.log(response.data);
});

async function search() {
  // regex for matching the ip address
  const ipRegex = /^\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}$/;

  if (ipRegex.test(inputData.value)) {
    try {
      // getting the ipAddress info
      const response = await axios.get(
        `https://geo.ipify.org/api/v2/country,city?apiKey=${apiKey}&ipAddress=${inputData.value}`
      );
      ipAddress.value = response.data.ip;
      location.value = `${response.data.location.city}, ${response.data.location.country}, ${response.data.location.geonameId}`;
      timezone.value = response.data.location.timezone;
      isp.value = response.data.isp;
      lat.value = response.data.location.lat;
      lng.value = response.data.location.lng;
      console.log(response.data);
      map.value.remove(); // clears the initiate map value for new values
      mapDraw();
    } catch (error) {
      console.error("Error fetching data:", error);
    }
  } else {
    try {
      // getting the ipAddress info
      const response = await axios.get(
        `https://geo.ipify.org/api/v2/country,city?apiKey=${apiKey}&domain=${inputData.value}`
      );
      ipAddress.value = response.data.ip;
      location.value = `${response.data.location.city}, ${response.data.location.country}, ${response.data.location.geonameId}`;
      timezone.value = response.data.location.timezone;
      isp.value = response.data.isp;
      lat.value = response.data.location.lat;
      lng.value = response.data.location.lng;
      console.log(response.data);
      map.value.remove(); // clears the initiate map value for new values
      mapDraw();
    } catch (error) {
      console.error("Error fetching data:", error);
    }
  }
}
</script>

<template>
  <header></header>
  <body>
    <div class="top-cover"></div>
    <div ref="mapContainer" class="map"></div>
    <div class="container">
      <main>
        <h1 class="text-center m-3">IP Address Tracker</h1>
        <div class="mx-auto" style="max-width: 40rem">
          <div class="input-group mb-3">
            <input
              v-model="inputData"
              type="text"
              placeholder="Search for any IP address or domain"
              class="form-control p-2"
              @keyup.enter="search()"
              aria-label="search-box"
              aria-describedby="button-addon2"
              style="
                border-top-left-radius: 0.7rem;
                border-bottom-left-radius: 0.7rem;
              "
            />
            <button
              @click="search()"
              class="btn btn-dark"
              type="button"
              style="
                border-top-right-radius: 0.7rem;
                border-bottom-right-radius: 0.7rem;
              "
            >
              <img src="../images/icon-arrow.svg" alt="icon-arrow" />
            </button>
          </div>
        </div>
        <div class="result card-group">
          <div
            class="card card-body text-bg-light mb-3 p-4 removeBorderTop"
            style=""
          >
            <h5 class="card-title">IP Address</h5>
            <p class="card-text">
              {{ ipAddress }}
            </p>
          </div>
          <div
            class="card card-body text-bg-light mb-3 p-4 removeBorder"
            style=""
          >
            <h5 class="card-title">Location</h5>
            <p class="card-text">
              {{ location }}
            </p>
          </div>
          <div
            class="card card-body text-bg-light mb-3 p-4 removeBorder"
            style=""
          >
            <h5 class="card-title">Timezone</h5>
            <p class="card-text">UTC {{ timezone }}</p>
          </div>
          <div
            class="card card-body text-bg-light mb-3 p-4 removeBorderBottom"
            style=""
          >
            <h5 class="card-title">ISP</h5>
            <p class="card-text">
              {{ isp }}
            </p>
          </div>
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
@media screen and (min-width: 576px) and (max-width: 992px) {
  .top-cover {
    background-size: contain;
    background-image: url("../images/pattern-bg-mobile.png") !important;
  }
}

@media (max-width: 575px) {
  main {
    padding: 1rem;
  }
  .top-cover {
    background-image: url("../images/pattern-bg-mobile.png") !important;
  }
  .map {
    position: fixed;
    top: 17.5rem !important;
    left: 0;
    width: 100%;
    height: 100%;
  }
  .card-body {
    margin: 0 !important;
    /* border: 0; */
    /* border-radius: 0; */
    text-align: center;
  }
  .removeBorder {
    margin-bottom: -1.6rem !important;
    border: 0;
    border-radius: 0;
  }
  .removeBorderTop {
    margin-bottom: -1.6rem !important;
    border-top-left-radius: 1rem;
    border-top-right-radius: 1rem;
    border-bottom-right-radius: 0;
    border-bottom-left-radius: 0;
    border-bottom: none;
  }
  .removeBorderBottom {
    border-bottom-left-radius: 1rem;
    border-bottom-right-radius: 1rem;
    border-top-right-radius: 0;
    border-top-left-radius: 0;
    border-top: none;
  }
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
.map {
  position: fixed;
  top: 12rem;
  left: 0;
  width: 100%;
  height: 100%;
}

.card-body p {
  font-weight: 500;
  font-size: 1.4rem;
  color: hsl(0, 0%, 17%);
}
.card-body h5 {
  font-size: 0.8rem;
  text-transform: uppercase;
  color: hsl(0, 0%, 59%);
}
.removeBorderTop {
  border-top-left-radius: 1rem;
  border-bottom-left-radius: 1rem;
}
.removeBorderBottom {
  border-top-right-radius: 1rem;
  border-bottom-right-radius: 1rem;
}
h1 {
  position: relative;
  color: white;
}
</style>
