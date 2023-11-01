<script setup lang="ts">
const props = defineProps(["rate"]);
</script>

<template>
  <p class="title">{{ props.rate[0].title }}</p>
  <p class="title">{{ props.rate[1] }}</p>
</template>

<style scoped>
p {
  margin: 0em;
}

.title {
  font-weight: bold;
  font-size: 1.2em;
}

menu {
  list-style-type: none;
  display: flex;
  flex-direction: row;
  gap: 1em;
  padding: 0;
  margin: 0;
}
</style>
