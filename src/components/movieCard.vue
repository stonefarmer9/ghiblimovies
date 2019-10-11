<template>
  <div class="film-container">
    <div class="title">
      <h1>{{ selectedMovie.title }}</h1>
    </div>
    <div class="description">
      {{ selectedMovie.description }}
    </div>
    <div class="card-container" :style="gridStyle">
      <div class="card">
        <h3>Directed by</h3>
        {{ selectedMovie.director }}
      </div>
      <div class="card">
          <h3>Produced by</h3>
        {{ selectedMovie.producer }}
      </div>
      <div class="card">
        <h3>Release Date</h3>
        {{ selectedMovie.release_date }}
      </div>
      <div class="card">
        <h3>Rotten Tomatoes Score</h3>
          {{ selectedMovie.rt_score }}
      </div>
      <div class="card">
        <ul v-if="characterList === []">
          <li v-for="character in characterList" :key="selectedMovie.id">
            {{ character.name }}
          </li>
        </ul>
        <div v-else>Sadly there are no characters listed for this film</div>
      </div>
      <div class="info-cards-container">
        <div class="info-card">
        </div>
      </div>
    </div>
</div>
  </div>
</template>


<script>
export default {
  name: 'movie-card',
  props: {
    selectedMovieId: String
  },
  data(){
    return {
      numberOfColumns: 4,
      selectedMovie: [],
      characterList: [],
    }
  },
  computed: {
    gridStyle() {
      return{
        gridTemplateColumns: `repeat(${this.numberOfColumns}, minmax(100px, 1fr))`
      }
    }
  },
  mounted() {
    this.getMovie(this.selectedMovieId)
  },
  methods: {
    async getMovie(id) {
      try {
        const response = await fetch(`https://ghibliapi.herokuapp.com/films/${id}`)
        const data = await response.json() //https://ghibliapi.herokuapp.com/films/0440483e-ca0e-4120-8c50-4c8cd9b965d6
        this.selectedMovie = data
        this.getCharacters(this.selectedMovie.people)
      } catch (error) {
        // eslint-disable-next-line
        console.error(error)
      }
    },
    async getCharacters(characterArray) {
      if( characterArray.length > 1){
        let promises = []
        let list = []
        characterArray.forEach(function(url){
          promises.push(fetch(url))
        })
        await Promise.all(promises)
          .then(
            values => Promise.all(
              values.map(value => value.json())
            ))
            .then(
              values => values.forEach(value => this.characterList.push(value)),
            )
      }
    }
  }
}
</script>


<style scoped>
.film-container {
  max-width: 1500px;
  margin: 0 auto;
}

.card-container {
  display: grid;
  grid-gap: 1em;
}

.card {
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

.card:hover {
  box-shadow: 0px 0px 85px 25px 20px;
  transform: scale(1.5)
}
.description {
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
.title {
  font-size: 1.7em;
  font-style: italic;
}


</style>
