<template>
  <div>
    <h1>{{ message }}</h1>
    <div>
      Search by title:
      <input type="text" v-model="titleFilter" list="titles" />
      <datalist id="titles">
        <option v-for="movie in movies" :key="movie.id">{{ movie.title }}</option>
      </datalist>
    </div>

    <div>
      <button v-on:click="setSortAttribute('title')">Sort by Title</button>
      <button v-on:click="setSortAttribute('actor')">Sort by Actor</button>
    </div>
    <div>
      <button>Sort Alphabetically</button>
    </div>
    <div
      v-for="movie in orderBy(filterBy(movies, titleFilter, 'title'), sortAttribute)"
      v-on:click="currentMovie = movie"
      v-bind:class="{ selected: movie === currentMovie }"
      :key="movie.id"
    ></div>
  </div>
</template>

<script>
import axios from "axios";
import moment from "moment";
import Vue2Filters from "vue2-filters";
export default {
  mixins: [Vue2Filters.mixin],
  data: function () {
    return {
      message: "Welcome to the Movie app!!",
      movies: [],
      currentMovie: {},
      titleFilter: "",
      sortAttribute: "title",
    };
  },
  created: function () {
    this.indexMovies();
  },
  methods: {
    indexMovies: function () {
      axios.get("/movies").then((response) => {
        this.movies = response.data;
        console.log("All movies", this.movies);
      });
    },
    relativeDate: function (date) {
      return moment(date).fromNow();
    },
    setSortAttribute: function (inputAttribute) {
      this.sortAttribute = inputAttribute;
    },
  },
};
</script>
