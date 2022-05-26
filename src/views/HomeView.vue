<template>
  <div class="flex flex-col h-screen max-h-screen">
    <!--Search / Result-->
    <div class="z-20 flex justify-center relative bg-hero-pattern bg-cover px-4 pt-8 pb-28">
      <div class="w-full max-w-screen-sm">
        <h1 class="text-white text-center font-bold text-3xl pb-8">Śledzenie adresu IP</h1>
        <div class="flex">
          <input 
          v-model="queryIp"
          class="flex-1 py-3 px-2 rounded-tl-md rounded-bl-md focus:outline-none" 
          type="text"
          placeholder="Wpisz adres IP lub zostaw puste żeby sprawdzić swój adres IP"
          />
          <i @click="getIpInfo" class="cursor-pointer bg-black text-white px-4 rounded-tr-md rounded-br-md flex items-center fa-solid fa-chevron-right"></i>
        </div>
      </div>
    <!-- IPInfo -->
    <IPInfo v-if="ipInfo" v-bind:ipInfo="ipInfo" />
    </div>
    <!-- Map -->
    <div id="map" class="h-full z-10"></div>
  </div>
</template>

<script>
// @ is an alias to /src
import IPInfo from "../components/IPInfo.vue";
import leaflet from "leaflet";
import { onMounted, ref } from "vue";
import axios from "axios";

export default {
  name: 'HomeView',
  components: {
    IPInfo
  },
  setup() {
    let map;
    const queryIp = ref("");
    const ipInfo = ref(null);

    onMounted(() => {
      map = leaflet.map('map').setView([50.033611, 22.004722], 13);

      leaflet.tileLayer('https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}?access_token=pk.eyJ1IjoiZmFqbnlnb3NjaXUiLCJhIjoiY2wzbHBtOWRlMXAyYjNkcXdpbzZyMGFyYiJ9.lx_JrjXsaLI4KUFO5mGR-Q', {
        attribution: 'Map data &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors, Imagery © <a href="https://www.mapbox.com/">Mapbox</a>',
        maxZoom: 18,
        id: 'mapbox/streets-v11',
        tileSize: 512,
        zoomOffset: -1,
        accessToken: 'pk.eyJ1IjoiZmFqbnlnb3NjaXUiLCJhIjoiY2wzbHBtOWRlMXAyYjNkcXdpbzZyMGFyYiJ9.lx_JrjXsaLI4KUFO5mGR-Q',
      })
      .addTo(map);
    });

    const getIpInfo = async () => {
        try {
          const data = await axios.get(`https://geo.ipify.org/api/v2/country,city?apiKey=at_hqSYpVIrX0WGnynbjp8LKxqnw1s6Z&ipAddress=${queryIp.value}`);
          const result = data.data;
          ipInfo.value = {
            address: result.ip,
            state: result.location.region,
            timezone: result.location.timezone,
            isp: result.isp,
            lat: result.location.lat,
            lng: result.location.lng,
          };
          leaflet.marker([ipInfo.value.lat, ipInfo.value.lng]).addTo(map);
          map.setView([ipInfo.value.lat, ipInfo.value.lng], 13);
        }
        catch (err) {
          alert(err.message);
        }
      }
      return { queryIp, ipInfo, getIpInfo };
  },
}
</script>
