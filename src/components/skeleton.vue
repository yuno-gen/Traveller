<template >
  <div>
    <q-card flat>
      <q-card-section class="text-h2 text-bold q-px-none">
        <tr>
          <th>
            <span class="text-orange-6">Explore </span>
          </th>
          <th class="text-right" style="width: 400px">
            <q-skeleton type="text" />
          </th>
        </tr>
      </q-card-section>
      <div class="row no-wrap">
        <div>
          <q-card-section class="q-pa-sm">
            <q-skeleton height="505px" width="500px" square />
          </q-card-section>
        </div>
        <div class="column">
          <div v-for="i in 2" :key="i">
            <q-card-section class="q-pa-sm">
              <q-skeleton height="250px" width="250px" square />
            </q-card-section>
          </div>
        </div>
      </div>
    </q-card>
    <div class="row">
      <div class="col-3 q-ma-sm" v-for="i in 3" :key="i">
        <div class="q-pt-md" v-for="j in 3" :key="j">
          <q-card class="mycard">
            <div>
              <q-skeleton width="100%" height="200px" square />
            </div>

            <q-card-section class="p-mt-xl">
              <div class="row no-wrap">
                <div class="col text-h6 ellipsis">
                  <q-skeleton square />
                </div>
              </div>
              <div class="text-subtitle1 text-caption">
                <q-skeleton type="text" />
              </div>
              <div>
                <q-skeleton type="text" />
              </div>
            </q-card-section>

            <q-card-section class="q-pt-none"> </q-card-section>
          </q-card>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    loadingPlaces: {
      type: Boolean,
      default: false,
    },
  },
};
</script>

<style>
</style>