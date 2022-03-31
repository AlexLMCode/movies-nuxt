<template>
  <main class="home">
    <!-- Hero -->
    <Hero />

    <!-- Movies -->
    <div class="container movies">
      <div id="movie-grid" class="movies-grid">
        <div v-for="movie in movies" :key="movie.id" class="movie">
          <MoviePoster :movie="movie" />
        </div>
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
    }
  },
  async fetch() {
    await this.getMovies()
  },
  methods: {
    async getMovies() {
      const { data } = await this.$axios.get(
        `/movie/now_playing?api_key=3437ff1f51a6aa288135641ecead7641&language=en-US&page=1`
      )
      data.results.forEach((movie) => {
        this.movies.push(movie)
      })
      // console.log(this.movies)
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
