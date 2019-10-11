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
    <div v-if="characterList.length > 0" class="card">
      <ul class="characterList">
        <li v-for="character in characterList" :key="character.id">
          {{ character.name }}
        </li>
      </ul>
    </div>
    <div v-else class="card">Sadly no data for this one </div>
    <button class="card" @click="$emit('close:movie')">CLOSE</button>
  </div>
</div>

</template>


<script>
export default {
  name: 'movie-card',
  props: {
    selectedMovieId: String,
    peopleLink: Array
  },
  data(){
    return {
      numberOfColumns: 4,
      selectedMovie: [],
      characterList: []
    }
  },
  computed: {
    gridStyle() {
      return{
        gridTemplateColumns: `repeat(${this.numberOfColumns}, minmax(100px, 1fr))`
      }
    }
  },
  beforeMount() {
    this.getMovie(this.selectedMovieId)
    this.getCharacters(this.peopleLink)
  },
  methods: {
    async getMovie(id) {
      try {
        const response = await fetch(`https://ghibliapi.herokuapp.com/films/${id}`)
        const data = await response.json()
        this.selectedMovie = data
      } catch (error) {
        // eslint-disable-next-line
        console.error(error)
      }
    },
    async getCharacters(characterArray) {
      if( characterArray.length > 1){
        let promises = []
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

.characterList {
  list-style: none;
}


</style>
