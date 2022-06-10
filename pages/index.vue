<template>
  <div class="home">
    <HeroComponent />
    <div class="container search">
      <input v-model.lazy="searchInput" type="text" placeholder="Search movies..." @keyup.enter="$fetch" />
      <button v-show="searchInput !== ''" class="button" @click="clearSearch">Clear Search</button>
    </div>

    <LoadingComponent v-if="$fetchState.pending" />

    <div v-else class="container movies">
      <MovieComponent v-if="searchInput !== ''" :movies="searchedMovies" />
      <MovieComponent v-else :movies="movies" />
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: "HomePage",

  data() {
    return {
      movies: [],
      searchedMovies: [],
      searchInput: ''
    }
  },

  async fetch() {
    if (this.searchInput === '') {
      await this.getMovie()
      return
    }
    await this.getSearchedMovie()
  },
  fetchDelay: 1000,
  head() {
    return {
      title: 'Movie App - Latest Streaming Movie Info',
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'Get all the latest streaming movies in theaters & online',
        },
        {
          hid: 'keywords',
          name: 'keywords',
          content: 'movies, stream, streaming',
        },
      ],
    }
  },
  methods: {
    async getMovie() {
      const response = await axios.get("https://api.themoviedb.org/3/movie/now_playing?api_key=2d092c05b6df26808e64c2b3859bdd49&language=en-US&page=1")
      response.data.results.forEach((result) => this.movies.push(result));
    },

    async getSearchedMovie() {
      this.searchedMovies = []
      const response = await axios.get(`https://api.themoviedb.org/3/search/movie?api_key=2d092c05b6df26808e64c2b3859bdd49&language=en-US&query=${this.searchInput}&page=1&include_adult=true`)
      response.data.results.forEach((result) => this.searchedMovies.push(result));
    },
    clearSearch() {
      this.searchInput = ''
      this.searchedMovies = []
    }
  }
}
</script>
<style lang="scss">
.loading {
  padding-top: 120px;
  align-items: flex-start;
}

.search {
  display: flex;
  padding: 32px 16px;

  input {
    max-width: 350px;
    width: 100%;
    padding: 12px 6px;
    font-size: 14px;
    border: none;

    &:focus {
      outline: none;
    }
  }

  .button {
    border-top-left-radius: 0;
    border-bottom-left-radius: 0;
  }
}
</style>