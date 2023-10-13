<template>
  <h1>Star Wars Info Center</h1>
  
  <div v-if="loadingPage">
    Loading...
  </div>
  <div v-else v-for="(planet, index) in planetList" :key="index" id="planet-cards">
    <PlanetCard :planet="planet"></PlanetCard>
  </div>

</template>

<script>
import PlanetCard from './components/PlanetCard.vue'

export default {
  name: 'App',
  components: {
    PlanetCard
  },
  data() {
      return {
        planetList: [],
        loadingPage: true
      }
    },
    methods: {
      async getData() {
        const res = await fetch("https://swapi.dev/api/planets/");
        const finalRes = await res.json();
        this.planetList = finalRes.results;
        this.loadingPage = false;
      }
    },
    mounted() {
      this.getData()
    }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #597999;
  margin: 30px;
}
#planet-cards {
  margin-bottom: 30px;
}
body, html {
  background-color: black
}
h1 {
  color: #FF3131;
  margin-bottom: 10px;
}
</style>
