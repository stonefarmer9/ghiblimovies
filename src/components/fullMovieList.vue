<template>

  <div id="fullMovieList" class="small-container">
    <h1>All Movies</h1>
    <div class="titleCards" :style="gridStyle">
      <button v-for="movie in allMovies" @click="$emit('show:movie', movie)" :key="movie.id" class="movieCard">
        {{ movie.title }}
    </button>
  </div>
</div>

</template>

<script>
export default {
  name: 'full-movie-list',
  data() {
    return {
      numberOfColumns: 4,
    }
  },
  props: {
    allMovies: Array
  },
  computed: {
    gridStyle() {
      return {
        gridTemplateColumns: `repeat(${this.numberOfColumns}, minmax(100px, 1fr))`
      }
    },
  }
}
</script>

<style scoped>

  .titleCards {
    display: grid;
    grid-gap: 1em;
  }

  .movieCard {
    margin: 1rem;
    padding: 1.5rem 2.5rem;
    font-size: 1.5em;
    font-weight: 245;
    background-image: linear-gradient(#ff9966 0%, #ffffcc 100%);
    margin: 0 0 2rem 0;
    box-shadow: 0px 0px 15px 10px  #b3c6ff;
    border-radius: 12px;
    transition: transform .8s;
  }

  .movieCard:hover {
    box-shadow: 0px 0px 85px 25px 20px #b3c6ff;
    transform: scale(1.5)
  }

  .small-container {
    max-width: 1500px;
    margin: 0 auto;

  }

  h1 {
      text-align: center;
    }

</style>
