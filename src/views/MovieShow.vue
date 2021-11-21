<template>
  <div class="movies-show">
    <h1>{{ movie.title }}</h1>
    <p>Title: {{ movie.title }}</p>
    <p>Year: {{ movie.year }}</p>
    <p>Plot: {{ movie.plot }}</p>
    <p>Director: {{ movie.director }}</p>
    <p>English: {{ movie.english }}</p>
    <div v-if="$parent.getUserId() == movie.user_id">
      <router-link v-bind:to="`/movies/${movie.id}/edit`">Edit this movie</router-link>
    </div>

    |
    <router-link to="/">Back to all movies</router-link>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      movie: {},
    };
  },
  created: function () {
    axios.get("/movies/" + this.$route.params.id).then((response) => {
      this.movie = response.data;
    });
  },
  methods: {},
};
</script>
