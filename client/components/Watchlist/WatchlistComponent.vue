<script setup lang="ts">
import { useUserStore } from "@/stores/user";
import { fetchy } from "@/utils/fetchy";
import { storeToRefs } from "pinia";
import { onBeforeMount, ref } from "vue";
import MovieComponent from "../Movie/MovieComponent.vue";

const { isLoggedIn } = storeToRefs(useUserStore());

const loaded = ref(false);
let movies = ref<Array<Record<string, string>>>([]);

async function getFavs() {
  let postResults;
  try {
    postResults = await fetchy("api/favorite", "GET", {});
  } catch (_) {
    return;
  }

  movies.value = postResults;
}

onBeforeMount(async () => {
  await getFavs();
  loaded.value = true;
});
</script>

<template>
  <section class="posts" v-if="loaded && movies.length !== 0">
    <article v-for="movie in movies" :key="movie._id">
      <MovieComponent :movie="movie" @refreshMovies="getFavs" />
    </article>
  </section>
  <p v-else-if="loaded">No posts found</p>
  <p v-else>Loading...</p>
</template>

<style scoped>
section {
  display: flex;
  flex-direction: column;
  gap: 1em;
}

section,
p,
.row {
  margin: 0 auto;
  max-width: 60em;
}

article {
  background-color: var(--base-bg);
  border-radius: 1em;
  display: flex;
  flex-direction: column;
  gap: 0.5em;
  padding: 1em;
}

.posts {
  padding: 1em;
}

.row {
  display: flex;
  justify-content: space-between;
  margin: 0 auto;
  max-width: 60em;
}
</style>
