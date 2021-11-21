<template>
  <div class="movies-new">
    <form v-on:submit.prevent="updateMovie()">
      <h1>Edit movie!</h1>
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      <div>
        <label>Title:</label>
        <input type="text" v-model="currentMovieParams.title" />
      </div>
      <div>
        <label>Year:</label>
        <input type="integer" v-model="currentMovieParams.year" />
      </div>
      <div>
        <label>Plot:</label>
        <input type="text" v-model="currentMovieParams.plot" />
      </div>
      <div>
        <label>Director:</label>
        <input type="text" v-model="currentMovieParams.director" />
      </div>
      <div>
        <label>English:</label>
        <input type="text" v-model="currentMovieParams.english" />
      </div>
      <input type="submit" value="Submit" />
    </form>
    <button v-on:click="destroyMovie()">Delete</button>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      currentmovieParams: {},
      errors: [],
    };
  },
  created: function () {
    axios.get(`/movies/${this.$route.params.id}`).then((response) => {
      console.log("movie Info:", response.data);
      this.currentMovieParams = response.data;
    });
  },
  methods: {
    updateMovie: function () {
      console.log("Update that movie!");
      axios
        .patch(`/movies/${this.$route.params.id}`, this.currentMovieParams)
        .then((response) => {
          this.$router.push(`/movies/${response.data.id}`);
        })
        .catch((error) => console.log(error.response));
    },
    destroyMovie: function () {
      axios.delete(`/movies/${this.$route.params.id}`).then((response) => {
        console.log("Good job!", response.data);
        this.$router.push("/movies");
      });
    },
  },
};
</script>
