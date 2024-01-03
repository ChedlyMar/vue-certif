<script setup>
// FROM LIBRARIES
import { computed, reactive, ref } from 'vue'
// COMPONENTS
import MovieItem from './components/MovieItem.vue'
import MovieForm from './components/MovieForm.vue'
// DATA
import { items } from './movies.json'

const movies = ref(items)

const validations = reactive({
  name: 'required',
  genres: 'required',
})

const showMovieForm = ref(false)

function showForm() {
  showMovieForm.value = true
}
function hideForm() {
  showMovieForm.value = false
}

function updateRating(movieIndex, rating) {
  movies.value[movieIndex].rating = rating
}
function removeMovie(movieIndex) {
  movies.value = movies.value.filter((movie, index) => index !== movieIndex)
}
function editMovie(movieIndex) {
  const movie = movies.value[movieIndex]

  form.id = movie.id
  form.name = movie.name
  form.description = movie.description
  form.image = movie.image
  form.inTheaters = movie.inTheaters
  form.genres = movie.genres

  showForm()
}

const validationRules = (rule) => {
  if (rule === 'required') return /^ *$/

  return null
}

function validate() {
  let valid = true
  clearErrors()
  // eslint-disable-next-line no-restricted-syntax
  for (const [field, rule] of Object.entries(validations)) {
    const validation = validationRules(rule)
    if (validation) {
      if (validation.test(form[field] || '')) {
        errors[field] = `${field} is ${rule}`
        valid = false
      }
    }
  }

  return valid
}

function updateMovie() {
  if (validate()) {
    const movie = {
      id: form.id,
      name: form.name,
      description: form.description,
      image: form.image,
      genres: form.genres,
      inTheaters: form.inTheaters,
      rating: 0,
    }

    movies.value = movies.value.map((m) => {
      if (m.id === movie.id) {
        movie.rating = m.rating
        return movie
      }
      return m
    })

    hideForm()
  }
}

function addMovie() {
  if (validate()) {
    const movie = {
      id: Number(Date.now()),
      name: form.name,
      description: form.description,
      image: form.image,
      genres: form.genres,
      inTheaters: form.inTheaters,
      rating: 0,
    }
    movies.value.push(movie)
    hideForm()
  }
}

function saveMovie() {
  if (form.id) {
    updateMovie()
  } else {
    addMovie()
  }
}

const averageRating = computed(() => {
  const avg = movies.value
    .map((movie) => parseInt(movie.rating || 0, 10))
    .reduce((a, b) => a + b, 0)

  return Number(avg / movies.value.length).toFixed(1)
})

const totalMovies = computed(() => movies.value.length)

function removeRatings() {
  movies.value = movies.value.map((movie) => {
    // eslint-disable-next-line no-param-reassign
    movie.rating = 0
    return movie
  })
}
</script>

<template>
  <div class="app">
    <div v-if="showMovieForm" class="modal-wrapper">
      <MovieForm
        :form="form"
        :errors="errors"
        @saveMovie="saveMovie"
        @hideForm="hideForm"
      />
      <!-- <div class="modal-wrapper-inner">
        <form @submit.prevent="saveMovie">
          <div class="movie-form-input-wrapper">
            <label for="name">Name</label>
            <input
              type="text"
              name="name"
              v-model="form.name"
              class="movie-form-input"
            />
            <span class="movie-form-error">{{ errors.name }}</span>
          </div>
          <div class="movie-form-input-wrapper">
            <label for="description">Description</label>
            <textarea
              type="text"
              name="description"
              v-model="form.description"
              class="movie-form-textarea"
            />
            <span class="movie-form-error">{{ errors.description }}</span>
          </div>
          <div class="movie-form-input-wrapper">
            <label for="image">Image</label>
            <input
              type="text"
              name="image"
              v-model="form.image"
              class="movie-form-input"
            />
            <span class="movie-form-error">{{ errors.image }}</span>
          </div>
          <div class="movie-form-input-wrapper">
            <label for="genre">Genres</label>
            <select
              name="genre"
              v-model="form.genres"
              class="movie-form-input"
              multiple
            >
              <option
                v-for="option in genres"
                :key="option.value"
                :value="option.value"
              >
                {{ option.text }}
              </option>
            </select>
            <span class="movie-form-error">
              {{ errors.genres }}
            </span>
          </div>
          <div class="movie-form-input-wrapper">
            <label for="genre" class="movie-form-checkbox-label">
              <input
                type="checkbox"
                v-model="form.inTheaters"
                :true-value="true"
                :false-value="false"
                class="movie-form-checkbox"
              />
              <span>In theaters</span>
            </label>
            <span class="movie-form-error">
              {{ errors.inTheaters }}
            </span>
          </div>
          <div class="movie-form-actions-wrapper">
            <button type="button" class="button" @click="hideForm">
              Cancel
            </button>

            <button type="submit" class="button-primary">
              <span v-if="form.id">Update</span>
              <span v-else>Create</span>
            </button>
          </div>
        </form>
      </div> -->
    </div>
    <div class="movie-actions-list-wrapper">
      <div class="movie-actions-list-info">
        <span>Total Movies: {{ totalMovies }}</span>
        <span> / </span>
        <span>Average Rating: {{ averageRating }}</span>
      </div>
      <div class="flex-spacer"></div>
      <div class="movie-actions-list-actions">
        <button
          class="self-end movie-actions-list-action-button button-primary justify-self-end"
          @click="removeRatings"
        >
          Remove Ratings
        </button>
        <button
          class="movie-actions-list-action-button"
          :class="{
            'button-primary': !showMovieForm,
            'button-disabled': showMovieForm,
          }"
          @click="showForm"
          :disabled="showMovieForm"
        >
          Add Movie
        </button>
      </div>
    </div>
    <div class="movie-list">
      <div
        class="movie-item group"
        v-for="(movie, movieIndex) in movies"
        :key="movie.id"
      >
        <MovieItem
          :movie="movie"
          :movieIndex="movieIndex"
          @updateRating="updateRating"
          @editMovie="editMovie"
          @removeMovie="removeMovie"
        />
      </div>
    </div>
  </div>
</template>
