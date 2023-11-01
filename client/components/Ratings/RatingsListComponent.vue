<script setup lang="ts">
import { useUserStore } from "@/stores/user";
import { fetchy } from "@/utils/fetchy";
import { storeToRefs } from "pinia";
import { onBeforeMount, ref } from "vue";
import RatingComponent from "./RatingComponent.vue";

const { isLoggedIn } = storeToRefs(useUserStore());

const loaded = ref(false);
let ratings = ref<Array<Record<string, string>>>([]);

async function getRatings() {
  let postResults;
  try {
    postResults = await fetchy("api/rating", "GET", {});
  } catch (_) {
    return;
  }
  ratings.value = postResults;
}

onBeforeMount(async () => {
  await getRatings();
  loaded.value = true;
});
</script>

<template>
  {{ ratings }}
  <section class="posts" v-if="loaded && ratings.length !== 0">
    <article v-for="movie in ratings" :key="movie._id">
      <RatingComponent :rate="movie" />
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
