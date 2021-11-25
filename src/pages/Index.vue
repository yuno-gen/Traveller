<template>
  <q-page class="flex column">
    <q-parallax :height="346" :speed="0.5">
      <template v-slot:media>
        <img src="https://static.tacdn.com/img2/brand/home/home1021_dt.webp" />
      </template>

      <template>
        <div class="" style="width: 600px">
          <!-- <q-input
        v-model="search"
        @keyup.enter="getDataBySearch"
        bg-color="white"
        placeholder=" Search"
        outlined
        rounded
        @input="getAutoComplete"
      >
        <template v-slot:append>
          <q-btn
            @click="getDataBySearch"
            to="/citysearchpage"
            round
            dense
            flat
            icon="search"
          />
        </template>
      </q-input>

      <br> -->

          <q-select
            :value="search"
            placeholder=" Search"
            bg-color="white"
            rounded
            outlined
            use-input
            hide-selected
            fill-input
            hide-dropdown-icon
            :options="autoList"
            @input-value="setSearch"
            @keyup.enter="getDataBySearch"
          >
            <template v-slot:no-option>
              <q-item>
                <q-item-section class="text-grey"> No results </q-item-section>
              </q-item>
            </template>

            <template v-slot:append>
              <q-btn
                @click="getDataBySearch"
                to="/citysearchpage"
                round
                dense
                flat
                icon="search"
              />
            </template>
          </q-select>
        </div>
      </template>
    </q-parallax>

    <!-- <div class="col q-pt-lg q-px-md">
      <q-input
        v-model="search"
        @keyup.enter="getDataBySearch"
        placeholder="Where Do You Want to go?"
        @input="getAutoComplete"
      >
        <template v-slot:before>
          <q-icon @click="getDataBySearch" name="my_location" />
        </template>

        <template v-slot:append>
          <q-btn
            @click="getDataBySearch"
            to="/citysearchpage"
            round
            dense
            flat
            icon="search"
          />
        </template>
      </q-input>
    </div> -->
  </q-page>
</template>

<script>
import { LocalStorage } from "quasar";
export default {
  name: "PageIndex",
  data() {
    return {
      search: "",
      travelData: null,
      searchInfo: [],
      autoList: [],
      travelInfo: [],
      lat: 21.1458,
      lon: 79.0882,
      apiUrl: "https://discover.search.hereapi.com/v1/discover",
      apiUrlAuto: "https://autocomplete.search.hereapi.com/v1/autocomplete",
      apiKey: "BPW-o8N1SNyob4s9eQhPlJsOuHEGSgcZOLrEWOh-0Nw",
    };
  },
  methods: {
    setSearch(val) {
      this.search = val;
      this.getAutoComplete();
    },
    getLocation() {
      this.$q.loading.show();

      if (this.$q.platform.is.electron) {
        this.$axios.get("https://freegeoip.app/json/").then((response) => {
          this.lat = response.data.latitude;
          this.lon = response.data.longitude;
          this.getWeatherByCoords();
        });
      } else {
        navigator.geolocation.getCurrentPosition((position) => {
          this.lat = position.coords.latitude;
          this.lon = position.coords.longitude;
          this.getWeatherByCoords();
        });
      }
    },
    getWeatherByCoords() {
      this.$q.loading.show();
      this.$axios(
        `${this.apiUrl}?lat=${this.lat}&lon=${this.lon}&appid=${this.apiKey}&units=metric`
      ).then((response) => {
        this.weatherData = response.data;
        this.$q.loading.hide();
      });
    },
    getAutoComplete() {
      this.autoList = [];
      if (this.search.length > 2) {
        this.$axios(
          `${this.apiUrlAuto}?q=${this.search}&apiKey=${this.apiKey}&units=metric`
        ).then((response) => {
          response.data.items.forEach((data) => {
            if (data.resultType != "street") {
              let autoData = {
                title: data.title,
                address: data.address,
              };
              this.autoList.push(autoData.address.label);
            }
          });
        });
      }
    },
    getDataBySearch() {
      if (this.search) {
        LocalStorage.set("searchName", this.search);
        this.$router.push("/citysearchpage");
      }
    },

    dataError() {
      this.$q.notify({
        message: "Unable to fetch data",
        color: "purple",
      });
      this.$q.dialog({
        title: "Error",
        message: "Could not find your location",
      });
    },
  },
};
</script>

<style lang="sass">
</style>
