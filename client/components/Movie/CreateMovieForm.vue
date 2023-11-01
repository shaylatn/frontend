<script setup lang="ts">
import { ref } from "vue";
import { fetchy } from "../../utils/fetchy";

const movieTitle = ref("");

const emit = defineEmits(["refreshMovies"]);

const createMovie = async (movieTitle: string) => {
  try {
    await fetchy("api/movie", "POST", {
      body: { title: movieTitle },
    });
  } catch (_) {
    return;
  }
  emit("refreshMovies");
  emptyForm();
};

const emptyForm = () => {
  movieTitle.value = "";
};
</script>

<template>
  <form @submit.prevent="createMovie(movieTitle)">
    <label for="movieTitle">Movie Title:</label>
    <textarea id="movieTitle" v-model="movieTitle" placeholder="Add a Movie" required> </textarea>
    <button type="submit" class="pure-button-primary pure-button">Add Movie</button>
  </form>
</template>

<style scoped>
form {
  background-color: var(--base-bg);
  border-radius: 1em;
  display: flex;
  flex-direction: column;
  gap: 0.5em;
  padding: 1em;
}

textarea {
  font-family: inherit;
  font-size: inherit;
  height: 6em;
  padding: 0.5em;
  border-radius: 4px;
  resize: none;
}
</style>
