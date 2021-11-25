<template>
  <div>
    <transition
      appear
      enter-active-class="animated backInLeft"
      leave-active-class="animated backOutLeft"
    >
      <WeatherData
        :search="this.search"
        :showBanner="showBanner"
        :weatherInfo="this.weatherInfo[0]"
        @closeBanner="showBanner = false"
      />
    </transition>
    <q-page class="constrain q-pa-md">
      <div class="row q-gutter-xl justify-center">
        <q-btn
          color="white"
          text-color="black"
          outline
          icon-right="mail"
          :ripple="{ color: 'blue' }"
          label="All"
          @click="allData"
        />

        <q-btn
          color="white"
          text-color="black"
          outline
          icon-right="hotel"
          :ripple="{ color: 'blue' }"
          label="Hotels"
          @click="getHotels"
        />

        <q-btn
          color="white"
          text-color="black"
          outline
          :ripple="{ color: 'blue' }"
          icon-right="restaurant"
          label="Restaurant"
          @click="getRestaurants"
        />
        <q-btn
          color="white"
          text-color="black"
          outline
          :ripple="{ color: 'blue' }"
          icon-right="tour"
          label="Activites"
          @click="getAttractions"
        />
        <q-btn
          color="white"
          text-color="black"
          outline
          icon-right="mail"
          :ripple="{ color: 'blue' }"
          label="Suggestions"
          @click="showRoute = true"
        />
      </div>
      <skeleton v-if="loadingPlaces" :loadingPlaces="loadingPlaces" />
      <div v-else>
        <q-card flat>
          <q-card-section class="text-h2 text-bold q-px-none">
            <span class="text-orange-6">Explore </span>
            {{ search }}
          </q-card-section>
          <div v-if="searchData[0]">
            <q-card class="constrain" flat v-if="searchData[0]"
              ><q-card-section>
                <div class="row no-wrap">
                  <div>
                    <q-card-section class="q-pa-sm">
                      <img
                        style="height: 505px; width: 500px"
                        :src="images[0].images.large.url"
                        alt=""
                      />
                    </q-card-section>
                  </div>

                  <div class="column">
                    <div v-for="i in 2" :key="i">
                      <q-card-section class="q-pa-md">
                        <img
                          style="height: 250px; width: 250px"
                          :src="images[i].images.medium.url"
                          alt=""
                        />
                      </q-card-section>
                    </div>
                  </div>
                </div>
              </q-card-section>
            </q-card>
          </div>
        </q-card>
        <div class="row">
          <div
            class="col-3 q-ma-sm"
            v-for="searches in searchData"
            :key="searches.id"
          >
            <div v-if="searches" class="q-pt-md">
              <q-card class="mycard">
                <div v-if="searches.result_object.photo">
                  <q-img
                    style="height: 200px"
                    :src="searches.result_object.photo.images.small.url"
                  />
                </div>
                <div v-else>
                  <q-img src="https://i.imgur.com/IIvismu.jpeg" />
                </div>
                <q-card-section>
                  <div class="row no-wrap">
                    <div class="col text-h6 ellipsis">
                      {{ searches.result_object.name }}
                    </div>
                  </div>
                  <div class="text-subtitle1 text-caption">
                    {{ searches.result_object.address }}
                  </div>
                  <div>
                    <q-rating
                      :value="parseInt(searches.result_object.rating, 10)"
                      :max="5"
                      size="32px"
                    />
                  </div>
                </q-card-section>

                <q-card-section class="q-pt-none"> </q-card-section>
              </q-card>
            </div>
          </div>
        </div>
      </div>

      <card-for-data
        :showRoute="showRoute"
        :suggestedRoute="this.suggestedRoute"
        @closeDialog="showRoute = false"
      />
    </q-page>
  </div>
</template>

<script>
import CardForData from "components/CardForData.vue";
import WeatherData from "../components/WeatherBanner.vue";
import Skeleton from "components/skeleton.vue";
import { LocalStorage } from "quasar";
export default {
  name: "SearchCityPage",
  components: {
    CardForData,
    WeatherData,
    Skeleton,
  },
  data() {
    return {
      search: "",
      showRoute: false,
      loadingPlaces: false,
      showBanner: true,
      travelData: null,
      showHotels: true,
      showRes: true,
      showAttraction: true,
      searchInfo: {},
      Preferences: [],
      restaurants: [],
      suggestedRestaurants: [],
      hotels: [],
      selectedHotel: {},
      weatherInfo: [],
      attractions: [],
      travelInfo: [],
      searchData: [],
      suggestedRoute: {},
      images: [],
      lat: 21.1458,
      lon: 79.0882,
      apiUrl: "https://discover.search.hereapi.com/v1/discover",
      apiUrlAuto: "https://autocomplete.search.hereapi.com/v1/autocomplete",
      apiKey: "BPW-o8N1SNyob4s9eQhPlJsOuHEGSgcZOLrEWOh-0Nw",
    };
  },
  computed: {},
  methods: {
    allData() {
      this.searchData = this.Preferences;
    },
    async getPreferences(preference) {
      this.$axios(
        `${this.apiUrl}?at=${this.lat},${this.lon}&q=${preference}&apiKey=${this.apiKey}&units=metric`
      )
        .then((response) => {
          //console.log('response.data: ', response.data);
          response.data.items.forEach((data) => {
            let preferData = {
              title: data.title,
              resultType: data.resultType,
              address: data.address,
              position: data.position,
            };
            if (preference == "restaurants") {
              this.restaurants.push(preferData);
            }
            if (preference == "hotels") {
              this.hotelPrefer.push(preferData);
            }
            if (preference == "attractions") {
              this.attractions.push(preferData);
            }
          });
        })
        .catch((err) => {
          console.log("error : ", err);
          this.dataError();
        });
    },
    async showingData(show) {
      this.showHotels = false;
      this.showRes = false;
      this.showAttraction = false;
      if (show == "hotels") {
        this.showHotels = true;
      }
      if (show == "restaurants") {
        this.showRes = true;
      }
      if (show == "attractions") {
        this.showAttraction = true;
      }
    },
    async getData() {
      var options = {
        method: "GET",
        url: "https://travel-advisor.p.rapidapi.com/locations/search",
        params: {
          query: this.search,
          limit: "30",
          offset: "0",
          units: "km",
          location_id: "1",
          currency: "USD",
          sort: "relevance",
          lang: "en_US",
        },
        headers: {
          "x-rapidapi-host": "travel-advisor.p.rapidapi.com",
          "x-rapidapi-key":
            "bf3cab535emsh3ccfb8412c87171p1c359ejsn0c463d05a172",
        },
      };

      await this.$axios
        .request(options)
        .then((response) => {
          this.searchData = [];
          this.searchData = response.data.data;
          this.Preferences = this.searchData;

          this.searchData.forEach((data) => {
            this.images.push(data.result_object.photo);
          });
          // this.loadingPlaces = false;
          // console.log("response:", this.searchData);
          this.getBestHotel();
        })
        .catch(function (error) {
          console.log("Error: ", error);
          // this.loadingPlaces = false;
        });
    },
    async getHotels() {
      this.searchData = this.Preferences;
      this.searchData = this.searchData.filter(
        (hotel) =>
          hotel.result_type === "lodging" || hotel.result_type === "geos"
      );
    },
    async getAttractions() {
      this.searchData = this.Preferences;
      this.searchData = this.searchData.filter(
        (attract) =>
          attract.result_type === "things_to_do" ||
          attract.result_type === "geos"
      );
    },
    async getRestaurants() {
      this.searchData = this.Preferences;
      this.searchData = this.searchData.filter(
        (hotel) =>
          hotel.result_type === "restaurants" || hotel.result_type === "geos"
      );
    },
    async getWeather() {
      // console.log("hekk", this.search);
      var options = {
        method: "GET",
        url: "https://weatherapi-com.p.rapidapi.com/forecast.json",
        params: { q: this.search, days: "8" },
        headers: {
          "x-rapidapi-host": "weatherapi-com.p.rapidapi.com",
          "x-rapidapi-key":
            "bf3cab535emsh3ccfb8412c87171p1c359ejsn0c463d05a172",
        },
      };

      this.$axios
        .request(options)
        .then((response) => {
          // console.log("response", response, options);

          this.weatherInfo = [];
          response.data.forecast.forecastday.forEach((data) => {
            let weather = {
              date: data.date,
              day: data.day,
              hour: data.hour,
            };

            // console.log("data:", weather);

            this.weatherInfo.push(weather);
          });
        })
        .catch(function (error) {
          console.error(error);
        });
    },
    async getDistanceFromLatLonInKm(lat1, lon1, lat2, lon2) {
      var R = 6371; // Radius of the earth in km
      var dLat = this.deg2rad(lat2 - lat1); // deg2rad below
      var dLon = this.deg2rad(lon2 - lon1);
      var a =
        Math.sin(dLat / 2) * Math.sin(dLat / 2) +
        Math.cos(this.deg2rad(lat1)) *
          Math.cos(this.deg2rad(lat2)) *
          Math.sin(dLon / 2) *
          Math.sin(dLon / 2);
      var c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a));
      var d = R * c; // Distance in km
      return d;
    },

    deg2rad(deg) {
      return deg * (Math.PI / 180);
    },

    async getBestHotel() {
      var suggestedAttraction = [];
      this.suggestedRestaurants = [];
      var suggestedHotels = [];
      this.attractions = [];
      this.restaurants = this.searchData.filter(
        (hotel) => hotel.result_type === "restaurants"
      );
      // console.log('restaurants: ', this.restaurants);
      this.hotels = this.searchData.filter(
        (hotel) => hotel.result_type === "lodging"
      );
      // console.log('hotels: ', this.hotels);
      this.attractions = this.searchData.filter(
        (attr) =>
          attr.result_type === "things_to_do" ||
          attr.result_type === "activities"
      );

      var pointOfRef = this.searchData[0];

      //this.attractions = this.attractions.sort(function(a, b){return a.rating-b.rating})
      // console.log('attractions: ', this.attractions)
      this.hotels.forEach((hotel) => {
        // console.log('localeCompare: ', hotel.result_object.rating.localeCompare("4.5"));
        suggestedHotels.push({
          distance: this.getDistanceFromLatLonInKm(
            pointOfRef.result_object.latitude,
            pointOfRef.result_object.longitude,
            hotel.result_object.latitude,
            hotel.result_object.longitude
          ),
          hotels: hotel,
        });
      });

      suggestedHotels = suggestedHotels.sort(function (a, b) {
        return a.distance - b.distance;
      });

      this.selectedHotel = suggestedHotels[0].hotels;

      // console.log('selectedHotel: ', this.selectedHotel);
      if (this.restaurants) {
        this.restaurants.forEach((restaurant) => {
          this.suggestedRestaurants.push({
            distance: this.getDistanceFromLatLonInKm(
              this.selectedHotel.result_object.latitude,
              this.selectedHotel.result_object.longitude,
              restaurant.result_object.latitude,
              restaurant.result_object.longitude
            ),
            restaurant: restaurant,
          });
        });
        this.suggestedRestaurants = this.suggestedRestaurants.sort(function (
          a,
          b
        ) {
          return a.distance - b.distance;
        });
      } else {
        this.$q.notify({
          message: "No restaurants found",
          color: $primary,
        });
      }
      if (this.attractions && this.suggestedRestaurants[0]) {
        this.attractions.forEach((attraction) => {
          suggestedAttraction.push({
            distance: this.getDistanceFromLatLonInKm(
              this.suggestedRestaurants[0].restaurant.result_object.latitude,
              this.suggestedRestaurants[0].restaurant.result_object.longitude,
              attraction.result_object.latitude,
              attraction.result_object.longitude
            ),
            attraction: attraction,
          });
        });
        suggestedAttraction = suggestedAttraction.sort(function (a, b) {
          return a.distance - b.distance;
        });
      }
      // console.log("suggestedAttraction: ", suggestedAttraction);
      this.suggestedRestaurants = this.suggestedRestaurants.sort(function (
        a,
        b
      ) {
        return a.distance - b.distance;
      });
      // console.log('suggestedRestaurants: ', );

      this.suggestedRoute = {
        hotel: this.selectedHotel,
        restaurant: this.suggestedRestaurants[0],
        attraction: suggestedAttraction[0],
      };
      // console.log("suggestedRoute: ", this.suggestedRoute);
    },
  },
  async created() {
    this.search = LocalStorage.getItem("searchName");
    this.loadingPlaces = true;
    // await this.getDataBySearch();
    if (this.search) {
      await this.getWeather();
      await this.getData();
    }

    this.loadingPlaces = false;
  },
  beforeDestroy() {
    LocalStorage.remove("searchName");
  },
};
</script>

<style>
.mycard {
  width: 100%;
  height: 400px;
  max-width: 280px;
  border-radius: 10px;
}
</style>