<template>
  <div class="home">
    <HeroComponent />

    <MovieComponent :movies="movies" />
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: "HomePage",

  data() {
    return {
      movies: [],
    }
  },
  async fetch() {
    await this.getMovie()
  },
  methods: {
    async getMovie() {
      const response = await axios.get("https://api.themoviedb.org/3/movie/now_playing?api_key=2d092c05b6df26808e64c2b3859bdd49&language=en-US&page=1")
      response.data.results.forEach((result) => this.movies.push(result));
    }
  }
}
</script>
