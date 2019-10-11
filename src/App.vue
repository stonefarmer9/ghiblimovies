<template>
  <div id="app" class="container">
    <img alt="Vue logo" src="./assets/ghibli.png">
    <FullMovieList v-if="selectedMovie === null"
      :allMovies="allMovies"
      @show:movie="showMovie"
    />
    <MovieCard v-else
      :selectedMovieId="selectedMovie.id"
      :peopleLink="selectedMovie.people"
      @close:movie="closeMovie"
    />
  </div>
</template>

<script>
import FullMovieList from '@/components/fullMovieList.vue'
import MovieCard from '@/components/movieCard.vue'

export default {
  name: 'app',
  components: {
    FullMovieList,
    MovieCard,
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
      try {
        const response = await fetch('https://ghibliapi.herokuapp.com/films')
        const data = await response.json()
        this.allMovies = data
        console.log(this.allMovies);
        // eslint-disable-next-line
      } catch (error){
        // eslint-disable-next-line
        console.error(error)
      }
    },
    showMovie(movie) {
      this.selectedMovie = movie

    },
    closeMovie(){
      this.selectedMovie = null
  }

  },
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
