<template>
  <div>
    <h1>Movie List</h1>
    <div v-if="error">{{ error }}</div>
    <div v-else-if="movies.length === 0">Loading...</div>
    <div v-else>
      <div v-for="(movie, index) in movies" :key="index" class="movie">
        <h3>{{ movie.title }}</h3>
        <div v-html="movie.description"></div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

const movies = ref([]);
const error = ref(null);

onMounted(async () => {
  try {
    const res = await axios.get('https://dev-movies-app-for-vue.pantheonsite.io/api/movies?_format=json');
    movies.value = res.data.map(movie => ({
      title: movie.title?.[0]?.value || '(No title)',
      description: movie.field_movie_summary?.[0]?.value || '(No description)',
    }));
  } catch (err) {
    error.value = 'Failed to load movies.';
    console.error(err);
  }
});
</script>

<style scoped>
.movie {
  margin-bottom: 20px;
  padding-bottom: 10px;
  border-bottom: 1px solid #ccc;
}
</style>
