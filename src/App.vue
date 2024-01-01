<script setup>
import { ref, watch } from 'vue'
import { items } from './movies.json'

const movieList = ref(items)
const newMovie = ref({
  id: 0,
  name: '',
  description: '',
  image: '',
  genre: [],
  inTheaters: false,
  rating: 3,
})

const totalMoviesNumber = ref(0)

const averageRationg = ref(0)

watch(
  movieList,
  () => {
    averageRationg.value =
      movieList.value.reduce((acc, curr) => acc + curr.rating, 0) /
      movieList.value.length
    totalMoviesNumber.value = movieList.value.length
  },
  { immediate: true }
)

const resetRatingToZero = () => {
  movieList.value = movieList.value.map((movie) => ({ ...movie, rating: 0 }))
}

const genreList = ref(['Drama', 'Action', 'Crime', 'Comedy'])

const setRating = (id, rating) => {
  movieList.value = movieList.value.map((movie) =>
    movie.id === id ? { ...movie, rating } : movie
  )
}

const addGenre = (genre) => {
  const index = newMovie.value.genre.findIndex(
    (existingGenre) => genre === existingGenre
  )
  if (index === -1) newMovie.value.genre.push(genre)
  else newMovie.value.genre.splice(index, 1)
}

const open = ref(false)
const closeModal = () => {
  newMovie.value = {
    id: 0,
    name: '',
    description: '',
    image: '',
    genre: [],
    inTheaters: false,
    rating: 3,
  }
  open.value = false
}

const updateMovie = () => {
  movieList.value = movieList.value.map((movie) =>
    movie.id === newMovie.value.id ? newMovie.value : movie
  )
}

const saveMovie = () => {
  if (newMovie.value.id === 0) {
    newMovie.value.id = Math.floor(Math.random() * 9999)
    movieList.value.push(newMovie.value)
  } else updateMovie()
  closeModal()
}

const editMovie = (movie) => {
  open.value = true
  newMovie.value = { ...movie }
}

const deleteMovie = (id) => {
  movieList.value = movieList.value.filter((movie) => movie.id !== id)
}
</script>

<template>
  <h1 class="page-title">app-certif</h1>
  <h2>Avreage rating: {{ averageRationg }}</h2>
  <h2>Movies total number: {{ totalMoviesNumber }}</h2>
  <button @click="open = true">Open Modal</button>
  <button @click="resetRatingToZero">reset rating</button>

  <Teleport to="body">
    <div v-if="open" class="modal">
      <div>
        <label for="">Name</label>
        <input type="text" v-model="newMovie.name" />
      </div>
      <div>
        <label for="">Description</label>
        <textarea rows="4" v-model="newMovie.description"></textarea>
      </div>
      <div>
        <label for="">image</label>
        <input type="text" v-model="newMovie.image" />
      </div>
      <div>
        <label for="">Genre</label>
        <select multiple>
          <option
            v-for="genre in genreList"
            :key="genre"
            @click="addGenre(genre)"
          >
            {{ genre }}
          </option>
        </select>
      </div>
      <div>
        <input type="checkbox" v-model="newMovie.inTheaters" />
        <label for="">In theaters</label>
      </div>
      <div>
        <button @click="closeModal">Cancel</button>
        <button @click="saveMovie">Save</button>
      </div>
    </div>
  </Teleport>

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
    <div class="movie-image-container">
      <span class="movie-image-rating">{{
        movie.rating ? movie.rating : '-'
      }}</span>
      <img class="movie-image" :src="movie.image" width="200" />
    </div>
    <button @click="deleteMovie(movie.id)">Delete</button>
    <button @click="editMovie(movie)">Edit</button>
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

.movie-image-rating {
  position: absolute;
  top: 0;
  right: 0;
  margin-right: 10px;
  color: red;
  font-weight: 900;
  font-size: 24px;
}

.movie-image-container {
  position: relative;
  width: 200px;
  margin-left: auto;
  margin-right: auto;
}

.modal {
  position: fixed;
  z-index: 999;
  top: 20%;
  left: 50%;
  width: 300px;
  margin-left: -150px;
  background-color: royalblue;
}
</style>
