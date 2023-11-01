<script setup lang="ts">
import { useUserStore } from "@/stores/user";
import { fetchy } from "@/utils/fetchy";
import { storeToRefs } from "pinia";
import { onBeforeMount, ref } from "vue";
import MovieComponent from "./MovieComponent.vue";

const { isLoggedIn } = storeToRefs(useUserStore());

const loaded = ref(false);
let movies = ref<Array<Record<string, string>>>([]);

async function getMovies() {
  // let query: Record<string, string> = author !== undefined ? { author } : {};
  let postResults;
  try {
    postResults = await fetchy("api/movie", "GET", {});
  } catch (_) {
    return;
  }
  movies.value = postResults;
}

async function addToFav(movie: string) {
  const fav = await fetchy("api/favorite", "POST", { body: { item: movie } });
  console.log(fav);
}

const rating = ref("");
const emit = defineEmits(["refreshRatings"]);

const createRating = async (rating: string, movie: string) => {
  try {
    await fetchy("api/rating", "POST", {
      body: { rating: rating, movie: movie },
    });
  } catch (_) {
    return;
  }
  emit("refreshRatings");
  emptyForm();
};
const emptyForm = () => {
  rating.value = "";
};
onBeforeMount(async () => {
  await getMovies();
  loaded.value = true;
});
</script>

<template>
  <!-- <section v-if="isLoggedIn">
    <h2>Add a movie:</h2>
    <CreateMovieForm @refreshMovies="getMovies" />
  </section> -->
  <section class="posts" v-if="loaded && movies.length !== 0">
    <article v-for="movie in movies" :key="movie._id">
      <MovieComponent :movie="movie" @refreshMovies="getMovies" />
      <form @submit.prevent="createRating(rating, movie._id)">
        <label for="content">Add Rating:</label>
        <textarea id="content" v-model="rating" placeholder="1-10" required> </textarea>
        <button class="rate">Rate Movie</button>
      </form>

      <button class="rate" @click="addToFav(movie._id)">Add to Watchlist</button>
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
textarea {
  font-family: inherit;
  font-size: inherit;
  height: 1em;
  width: 3em;
  padding: 0.5em;
  border-radius: 4px;
  resize: none;
}
</style>
