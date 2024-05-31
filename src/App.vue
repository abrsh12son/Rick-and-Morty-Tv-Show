<template>
    <navigation />
    <!-- <Carousel /> -->
    <RouterView />
  </template>
  
  <script setup>
  import { RouterView } from "vue-router";

  import navigation from "./components/navigation.vue";
// import Carousel from "./components/carousel.vue";
  </script>
  