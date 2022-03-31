<template>
  <main class="home">
    <!-- Hero -->
    <Hero />

    <!-- Search -->

    <Search
      :searchVal="searchInput"
      @searchValue="getSearchValue"
      @clearSearch="clear"
    />

    <!-- Loading -->
    <Loading v-if="$fetchState.pending" />

    <!-- Movies -->
    <div v-else class="container movies">
      <div v-if="searchInput !== ''" id="movie-grid" class="movies-grid">
        <!-- Searched Movies -->
        <section v-for="movie in searchedMovies" :key="movie.id" class="movie">
          <MoviePoster :movie="movie" />
        </section>
      </div>

      <div v-else id="movie-grid" class="movies-grid">
        <!-- Now Playing -->
        <section v-for="movie in movies" :key="movie.id" class="movie">
          <MoviePoster :movie="movie" />
        </section>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      movies: [],
      searchInput: '',
      searchedMovies: [],
    }
  },
  async fetch() {
    if (this.searchInput === '') {
      await this.getMovies()
      return
    }
    if (this.searchInput !== '') {
      // this.clearInput()
      await this.searchMovies()
    }
  },
  head() {
    return {
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'See all the latest streaming movies in theaters & online',
        },
        {
          hid: 'keywords',
          name: 'keywords',
          content: 'movies, stream, streaming',
        },
      ],
    }
  },
  fetchDelay: 500,
  methods: {
    async getMovies() {
      const { data } = await this.$axios.get(
        `/movie/now_playing?api_key=${process.env.apiKey}&language=en-US&page=1`
      )
      data.results.forEach((movie) => {
        this.movies.push(movie)
      })
    },
    async searchMovies() {
      const { data } = await this.$axios.get(
        `/search/movie?api_key=${process.env.apiKey}&language=en-US&query=${this.searchInput}&page=1&include_adult=false`
      )
      data.results.forEach((movie) => {
        this.searchedMovies.push(movie)
      })
    },
    getSearchValue(value) {
      this.searchInput = value
      this.$fetch()
    },
    clear(value) {
      this.searchInput = ''
      this.searchedMovies = []
    },
  },
}
</script>

<style lang="scss" scoped>
.home {
  .loading {
    padding-top: 120px;
    align-items: flex-start;
  }
  .movies {
    padding: 32px 16px;
    .movies-grid {
      display: grid;
      column-gap: 32px;
      row-gap: 64px;
      grid-template-columns: 1fr;
      @media (min-width: 500px) {
        grid-template-columns: repeat(2, 1fr);
      }
      @media (min-width: 750px) {
        grid-template-columns: repeat(3, 1fr);
      }
      @media (min-width: 1100px) {
        grid-template-columns: repeat(4, 1fr);
      }
    }
  }
}
</style>
