<template>
  <div class="home">
    <h1>Movies!</h1>

    <h1>New Movie</h1>
    Title:
    <input type="text" v-model="newMovieParams.title" />
    Year:
    <input type="integer" v-model="newMovieParams.year" />
    Director:
    <input type="text" v-model="newMovieParams.director" />
    Plot:
    <input type="text" v-model="newMovieParams.plot" />

    <button v-on:click="createMovie()">Add a movie.</button>

    <h1>Here are the movies:</h1>
    <div v-for="movie in movies" v-bind:key="movie.id">
      <h2>{{ movie.title }}</h2>
      <button v-on:click="showMovies(movie)">More Info</button>
    </div>
    <h1>Movie Info:</h1>
    <p>
      Title:
      <input type="text" v-model="currentMovie.title" />
    </p>
    <p>
      Year:
      <input type="integer" v-model="currentMovie.year" />
    </p>
    <p>
      Director:
      <input type="text" v-model="newMovieParams.director" />
    </p>
    <p>
      Plot:
      <input type="text" v-model="currentMovie.plot" />
    </p>

    <button v-on:click="updateMovie(currentMovie)">Update Movie</button>
    <button v-on:click="destroyMovie(currentMovie)">Delete</button>
    <button>close</button>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to my movies!",
      movies: [],
      newMovieParams: {},
      currentMovie: {},
    };
  },
  created: function () {},
  methods: {
    indexMovies: function () {
      axios.get("/movies").then((response) => {
        this.movies = response.data;
        console.log("All movies", this.movies);
      });
    },
    createMovie: function () {
      console.log("Enter a new movie!");
      axios
        .post("/movies", this.newMovieParams)
        .then((response) => {
          console.log("Success!", response.data);
          this.movies.push(response.data);
        })
        .catch((error) => console.log(error.response));
    },
    showMovie: function (movie) {
      console.log(movie);
      this.currentMovie = movie;
      document.querySelector("#movie-details").showModal();
    },
    updateMovie: function (movie) {
      axios.patch("/movies/" + movie.id, movie).then((response) => {
        console.log("Success", response.data);
      });
    },
    destroymovie: function (movie) {
      axios.delete("/movies/" + movie.id).then((response) => {
        console.log("Success!", response.data);
        var index = this.movies.indexOf(movie);
        this.movies.splice(index, 1);
      });
    },
  },
};
</script>
