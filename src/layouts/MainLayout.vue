<template>
  <q-layout view="lHh Lpr lFf" class="constrain q-px-lg">

    <q-header class="constrain bg-white q-pa-none " >
    <div class="row justify-between">
    <div class="q-pt-sm ">
      <q-avatar class="q-pb-xl" size="5rem">
      <img src="logo.png">
    </q-avatar>
    <span class="text-h4 text-black"> Traveller</span>
    </div>
    <div class="q-pt-sm">
       <q-btn flat round color="black" icon="search" />
    </div>
    </div>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
export default {
  name: "MainLayout",

  data() {
    return {};
  },
};
</script>
