<template>
  <div id="app" class="container">
    <img alt="Vue logo" src="./assets/ghibli.png">
    <FullMovieList v-if="movie === null"
      :allMovies="allMovies"
      @show:movie="showMovie"
    />
    <MovieCard v-else
      :selectedMoviemovie="selectedMovie"
    />
  </div>
</template>

<script>
import FullMovieList from '@/components/fullMovieList.vue'

export default {
  name: 'app',
  components: {
    FullMovieList
  },
  data() {
    return {
      allMovies: [],
      selectedMovie: null,
    }
  },
  mounted(){
    this.getMovies()
  },
  methods: {
    async getMovies() {
      try{
        const response = await fetch('https://ghibliapi.herokuapp.com/films')
        const data = await response.json()
        this.allMovies = data
        // eslint-disable-next-line
        console.log("SUCCESS", data)
      } catch (error){
        // eslint-disable-next-line
        console.error(error)
      }
    },
    showMovie(movie) {
      console.log("I Got here", movie);
      this.selectedMovie = movie
      // eslint-disable-next-line
      console.log(this.movie);
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
