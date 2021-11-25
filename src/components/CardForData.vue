<template>
  <q-dialog v-model="showRoute">
    <q-card class="mycard">
      <q-card-section>
        <div class="text-h6">
          Suggestions for Day According to the distance .
        </div>
      </q-card-section>

      <q-card-section class="q-mx-xl">
        <div v-if="suggestedRoute">
          <div v-if="suggestedRoute.hotel">
            <span class="q-pa-md text-caption">Suggested Hotel</span>
            <div class="row">
              <q-avatar color="primary" text-color="white">1</q-avatar>
              <span class="text-subtitle1 q-mx-md q-pa-sm">{{
                suggestedRoute.hotel.result_object.name
              }}</span>
            </div>
            <q-separator size="5px" />
          </div>

          <div v-if="suggestedRoute.restaurant">
            <span class="q-pa-md text-caption">Suggested Restaurant</span>
            <div class="row">
              <q-avatar color="primary" text-color="white">2</q-avatar>
              <span class="text-subtitle1 q-mx-md q-pa-sm">{{
                suggestedRoute.restaurant.restaurant.result_object.name
              }}</span>
            </div>
            <div class="q-pa-sm">
              <q-separator size="5px" />
            </div>
          </div>
          <div v-if="suggestedRoute.attraction">
            <span class="q-pa-md text-caption">Suggested Things to do</span>
            <div class="row">
              <q-avatar color="primary" text-color="white">3</q-avatar>
              <span class="text-subtitle1 q-mx-md q-pa-sm">{{
                suggestedRoute.attraction.attraction.result_object.name
              }}</span>
            </div>
          </div>
        </div>
      </q-card-section>

      <q-card-actions align="right">
        <q-btn flat label="OK" color="primary" @click="$emit('closeDialog')" />
      </q-card-actions>
    </q-card>
  </q-dialog>
</template>

<script>
export default {
  props: {
    showRoute: {
      type: Boolean,
      default: false,
    },
    suggestedRoute: {
      type: Object,
      default: null,
    },
  },
  // created() {
  //   console.log("route", this.suggestedRoute);
  // },
};
</script>

<style>
.mycard {
  width: 60vw;
}
</style>