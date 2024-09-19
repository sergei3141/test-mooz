<template>
  <div class="main">
    <div v-if="isLoading" >
      <img src="../load.gif" alt="Loading..." class="loading-overlay" />
    </div>
    <div v-if="films.length > 0">
      <h3>Результатов по запросу "{{search}}": {{totalResults}}</h3>
      <ul class="movies">
        <li v-for="film in films" :key="film.imdbID" class="movie">
          <img :src="film.Poster === 'N/A' ? 'https://placehold.co/300x500' : film.Poster" alt="Постер фильма" class="movie__poster">
          <ul class="movie__info">
            <li><h2><b>Название:</b> {{ film.Title }}</h2></li>
            <li><h2><b>Год:</b> {{ film.Year }}</h2></li>
            <li><h2><b>Id:</b> {{ film.imdbID }}</h2></li>
            <li><h2><b>Тип:</b> {{ film.Type }}</h2></li>
          </ul>
        </li>
      </ul>
      <FilmsPagination :total-results="totalResults" @page-changed="fetchMovies" />
    </div>
    <h3 v-else>
      <div v-if="error">Измените параметры поиска!</div>
      <p>{{ error }}</p>
    </h3>
  </div>
</template>

<script>
import axios from 'axios';
import FilmsPagination from './FilmsPagination.vue';

const baseUrl = 'https://www.omdbapi.com/';
const apiKey = '8523cbb8';
const i = 'tt3896198'

export default {
  components: {
    FilmsPagination
  },
  props: {
    searchQuery: String,
  },
  data() {
    return {
      films: [],
      error: "",
      totalResults: 0,
      search: "",
      isLoading: false, 
    };
  },
  methods: {
    async fetchMovies(searchQuery, page = 1) {
      this.isLoading = true; 
      try {
        const response = await axios.get(baseUrl, {
          params: {
            apikey: apiKey,
            s: searchQuery || this.search,
            page: page,
            i: i
          }
        });
        if (response.data.Response === "True") {
          this.films = response.data.Search;
          this.search = searchQuery || this.search;
          this.totalResults = response.data.totalResults;
        } else {
          this.error = response.data.Error;
          this.films = [];
          this.totalResults = 0;
        }
      } catch (error) {
        console.error(error);
      } finally {
        this.isLoading = false; 
      }
    }
  },
  watch: {
    searchQuery() {
      console.log(this.searchQuery);
      this.fetchMovies(this.searchQuery);
    }
  }
};
</script>


<style lang="scss">

$primary-color: #fff;

ul,
ol {
  list-style: none;
  margin: 0;
  padding: 0;
}

.main {
  margin: auto;
  max-width: 1380px;
}

.movies {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
}

.loading-overlay {
  width: 100px;
  margin: auto;
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.movie {
  background-color: $primary-color;
  border-radius: 10px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  padding: 20px;
  width: 220px;

  &__info { 
    margin-top: 30px;

    ul {
      list-style: none;
      padding: 0;
    }

    li {
      display: flex;
    }

    h2 {
      margin: 0;
      font-weight: 300;
      font-size: 18px;
    }
  }

  &__poster { 
    width: 220px;
    height: 350px;
    object-fit: cover;
    border-radius: 10px;
  }
}

.loading-overlay {
  width: 100px;
    margin: auto;
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}

h3 {
  margin: 10px 30px;
}

</style>