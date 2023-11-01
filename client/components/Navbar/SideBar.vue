<script setup lang="ts">
import { computed } from "vue";
import { RouterLink, useRoute } from "vue-router";
const emit = defineEmits(["closeSidebar"]);

const currentRoute = useRoute();
const currentRouteName = computed(() => currentRoute.name);
</script>
<template>
  <div id="mySidenav" class="sidenav">
    <button class="btn-small pure-button" @click="emit('closeSidebar')">Close Sidebar</button>
    <br />
    <li>
      <RouterLink :to="{ name: 'Home' }" :class="{ underline: currentRouteName == 'Home' }"> Feed </RouterLink>
    </li>
    <br />
    <li>
      <RouterLink :to="{ name: 'Watchlist' }" :class="{ underline: currentRouteName == 'Watchlist' }"> Watchlist </RouterLink>
    </li>
    <br />
    <li>
      <RouterLink :to="{ name: 'Ratings' }" :class="{ underline: currentRouteName == 'Ratings' }"> Ratings </RouterLink>
    </li>
  </div>
</template>

<style>
.sidenav {
  height: 100%; /* 100% Full-height */
  width: 25%; /* 0 width - change this with JavaScript */
  position: fixed; /* Stay in place */
  z-index: 1; /* Stay on top */
  top: 0; /* Stay at the top */
  left: 0;
  background-color: gray; /* Black*/
  overflow-x: hidden; /* Disable horizontal scroll */
  padding-top: 60px; /* Place content 60px from the top */
  transition: 0.5s; /* 0.5 second transition effect to slide in the sidenav */
}
</style>
