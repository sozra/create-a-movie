<script setup>
import { ref } from "vue";
import { StarIcon } from "@heroicons/vue/24/solid";
import { items } from "./movies.json";
const movies = ref(items);
const form = ref({
  name: "",
  description: "",
  image: "",
  genres: [],
  isInTheater: false,
});
const isFormVisible = ref(false);


function showForm() {
  isFormVisible.value = true;
}

function cancelForm() {
  isFormVisible.value = false;
  form.value = {
    name: "",
    description: "",
    image: "",
    genres: [],
    isInTheater: false,
  };
}

function submitForm() {
  // validate
  if (!form.value.name || !form.value.genres.length) {
    return;
  }
  movies.value.push({
    id: movies.value.length + 1,
    name: form.value.name,
    description: form.value.description,
    image: form.value.image,
    genres: form.value.genres,
    isInTheater: form.value.isInTheater,
  });
  isFormVisible.value = false;
  form.value = {
    name: "",
    description: "",
    image: "",
    genres: [],
    isInTheater: false,
  };
}


function updateRating(movieIndex, rating) {
  movies.value[movieIndex].rating = rating;
}
</script>

<template>
  <div class="app">
    <button class="h-9 bg-lime-700 w-20 rounded-2xl text-white absolute z-10 right-9 top-9" @click="showForm">add</button>
    <div class="movie-list">
      <div class="movie-item" v-for="(movie, movieIndex) in movies" :key="movie.id">
        <div class="movie-item-image-wrapper">
          <div class="movie-item-star-wrapper">
            <StarIcon id="rating" class="movie-item-star-rating-icon"
              :class="[movie.rating ? 'text-yellow-500' : 'text-gray-500']" />
            <div class="movie-item-star-content-wrapper">
              <span v-if="movie.rating" id="rating-stars" class="movie-item-star-content-rating-rated">
                {{ movie.rating }}
              </span>
              <span v-else class="movie-item-star-content-rating-not-rated">
                -
              </span>
            </div>
          </div>
          <!-- <img :src="movie.image" class="movie-item-image" alt="" /> -->
          <img src="https://placehold.co/682x969/cyan/white" class="movie-item-image" alt="" />
        </div>

        <div class="movie-item-content-wrapper">
          <div class="movie-item-title-wrapper">
            <h3 class="movie-item-title">{{ movie.name }}</h3>
            <div class="movie-item-genres-wrapper">
              <span v-for="genre in movie.genres" :key="`${movie.id}-${genre}`" class="movie-item-genre-tag">{{ genre
              }}</span>
            </div>
          </div>
          <div class="movie-item-description-wrapper">
            <p class="movie-item-description">{{ movie.description }}</p>
          </div>
          <div class="movie-item-rating-wrapper">
            <span class="movie-item-rating-text">
              Rating: ({{ movie.rating }}/5)
            </span>

            <div class="movie-item-star-icon-wrapper">
              <button v-for="star in 5" :key="star" class="movie-item-star-icon-button" :class="[
                star <= movie.rating ? 'text-yellow-500' : 'text-gray-500',
              ]" :disabled="star === movie.rating" @click="updateRating(movieIndex, star)">
                <StarIcon class="movie-item-star-icon" />
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div :style="{ display: isFormVisible ? 'unset' : 'none' }"
      class="movie-form h-[600px] w-[600px] absolute self-center place-self-center bg-slate-300 p-9 box-content">
      <div class="w-full mb-4">
        <label for="movie-name">Name</label>
        <input v-model="form.name" class="w-full rounded-lg" type="text" name="movie-name" id="movie-name" required />
      </div>

      <div class="w-full mb-4">
        <label for="Description">Description</label>
        <textarea v-model="form.description" class="w-full rounded-lg" type="text" name="Description" id="Description"
          rows="5" cols="33"> </textarea>
      </div>

      <div class="w-full mb-4">
        <label for="movie-image">Image</label>
        <input v-model="form.image" class="w-full rounded-lg" type="text" name="movie-image" id="movie-image" />
      </div>

      <div class="w-full mb-4">

        <label for="movie-select">Genres</label>

        <select v-model="form.genres" class="w-full rounded-lg" name="genre" id="movie-select" multiple required>
          <option value="">--Please choose an option--</option>
          <option value="dog">Dog</option>
          <option value="cat">Cat</option>
          <option value="hamster">Hamster</option>
          <option value="parrot">Parrot</option>
          <option value="spider">Spider</option>
          <option value="goldfish">Goldfish</option>
        </select>
      </div>

      <div class="w-full flex items-center justify-center">
        <label for="is-in-theater">Is in theaters</label>
        <input v-model="form.isInTheater" type="checkbox" id="is-in-theater" name="is-in-theater" />
      </div>

      <div class="button-group flex justify-evenly">
        <button class="h-12 rounded-xl w-28 text-white bg-blue-500 hover:bg-blue-400" @click="submitForm">submit</button>
        <button class="h-12 rounded-xl w-28 text-white bg-green-500 hover:bg-green-400"
          @click="cancelForm">cancel</button>
      </div>
      <div>{{ form }} }}</div>
    </div>
  </div></template>
