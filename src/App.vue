<script setup>
import { ref } from 'vue'
import { items } from './movies.json'

const movieList = ref(items)

const setRating = (id, rating) => {
  console.log('test')
  movieList.value = movieList.value.map((movie) =>
    movie.id === id ? { ...movie, rating } : movie
  )
}
</script>

<template>
  <h1 class="page-title">app-certif</h1>
  <div v-for="movie in movieList" :key="movie.id">
    <h2 class="movie-title">{{ movie.name }}</h2>
    <p
      class="movie-genre"
      v-for="genre in movie.genres"
      :key="`${movie.id}${genre}`"
    >
      {{ genre }}
    </p>
    <p class="movie-description">{{ movie.description }}</p>
    <img class="movie-image" :src="movie.image" width="200" />
    <div class="movie-rating">
      <label class="movie-rating-label" for="">rating</label>
      <div
        class="movie-rating-stars"
        :class="i === movie.rating ? 'cursor-disabled' : 'cursor-pointer'"
        v-for="i in movie.rating"
        :key="`${movie.id}${i}`"
        @click="setRating(movie.id, i)"
      >
        *
      </div>
      <div
        class="movie-rating-stars cursor-pointer"
        v-for="i in 5 - movie.rating"
        :key="`${movie.id}${i}`"
        @click="setRating(movie.id, i)"
      >
        -
      </div>
    </div>
  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}

.cursor-pointer {
  cursor: pointer;
}
.cursor-disabled {
  cursor: not-allowed;
}
</style>
