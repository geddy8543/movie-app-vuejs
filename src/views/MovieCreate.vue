<template>
  <div class="movies-new">
    <form v-on:submit.prevent="createMovie()">
      <h1>New movie!</h1>
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      <div>
        <label>Title:</label>
        <input type="text" v-model="newMovieParams.title" />
      </div>
      <div>
        <label>Year:</label>
        <input type="integer" v-model="newMovieParams.year" />
      </div>
      <div>
        <label>Plot:</label>
        <input type="text" v-model="newMovieParams.plot" />
        <small v-if="newMovieParams.plot.length > 0 && newMovieParams.plot.length < 20" class="text-danger">
          Plot cannot exceed 500 characters
        </small>
      </div>
      <div>
        <label>Director:</label>
        <input type="text" v-model="newMovieParams.director" />
      </div>
      <div>
        <label>English:</label>
        <input type="text" v-model="newMovieParams.english" />
      </div>
      <input type="submit" value="Submit" />
    </form>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      newMovieParams: {},
      errors: [],
      status: "",
    };
  },
  methods: {
    createMovie: function () {
      console.log("Enter a new movie!");
      axios
        .post("/movies", this.newMovieParams)
        .then(() => {
          this.$router.push("/movies");
        })
        .catch((error) => {
          this.status = error.response.status;
          console.log(error.response);
        });
    },
  },
};
</script>
