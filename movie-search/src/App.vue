<template>
  <div class="App">
    <header class="App-header">
      <img src=
"@/assets/logo-1.png"
      alt="Logo"
      class="logo" />
      <h1>Movie Search Platform</h1>
    </header>
    <main>
      <MovieSearch @searchInput="searchInput"
                     @search="search" />
      <div class="container">
        <div
          v-for="movie in filteredResults"
          :key="movie.imdbID"
          class="item"
          @click="openDetail(movie.imdbID)"
        >
          <img :src="movie.Poster" alt="Movie Poster" />
          <h3>{{ movie.Title }}</h3>
        </div>
      </div>
      <MovieDetail v-if="selected.Title"
                     :selected="selected" 
                     @closeDetail="closeDetail" />
    </main>
  </div>
</template>

<script>
import axios from "axios";
import MovieSearch from "./components/MovieSearch.vue";
import MovieDetail from "./components/MovieDetail.vue";

export default {
  name: "App",
  components: {
    MovieSearch,
    MovieDetail,
  },
  data() {
    return {
      s: "", // Initialize search query
      results: [], // Initialize results array
      selected: {}, // Initialize selected movie object
      apiurl: "https://www.omdbapi.com/?apikey=a2526df0",
    };
  },
  mounted() {
    // Fetch initial movies based on specific keywords
    this.fetchInitialMovies();
  },
  methods: {
    fetchInitialMovies() {
      // Keywords for initial movie search
      const keywords = ["hindi", "avengers", "comedy"];
      
      // Fetch movies for each keyword 
      // and combine the results
      Promise.all(keywords.map(keyword => {
        return axios(this.apiurl + "&s=" + keyword)
          .then(({ data }) => data.Search || [])
          .catch(error => {
            console.error("Error fetching movies:", error);
            return [];
          });
      })).then(results => {
        this.results = results.flat(); 
        // Combine arrays of movies
      });
    },
    searchInput(e) {
      this.s = e.target.value;
    },
    search(e) {
      if (e.key === "Enter") {
        axios(this.apiurl + "&s=" + this.s)
             .then(({ data }) => {
          this.results = data.Search || [];
        });
      }
    },
    openDetail(id) {
      axios(this.apiurl + "&i=" + id).then(({ data }) => {
        this.selected = data;
      });
    },
    closeDetail() {
      this.selected = {};
    },
  },
  computed: {
    filteredResults() {
      // Filter results based on search query
      return this.results.filter(movie => movie.Title.toLowerCase()
                                  .includes(this.s.toLowerCase()));
    }
  },
};
</script>



<style>
.App {
  text-align: center;
  background-color: #f4f4f4;
  min-height: 100vh;
}

.App-header {
  background-color: black;
  color: white;
  padding: 20px 0;
  display: flex;
  justify-content: center;
  align-items: center;
}

.logo {
  width: 50px;
  margin-right: 10px;
}

.container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
  padding: 20px;
}

.item {
  cursor: pointer;
  width: 200px;
  text-align: center;
  padding: 10px;
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease;
}

.item:hover {
  transform: translateY(-5px);
}

.item img {
  width: 100%;
  border-radius: 8px;
}

.MovieDetail {
  background-color: rgba(0, 0, 0, 0.8);
  color: white;
  padding: 20px;
  border-radius: 8px;
  max-width: 600px;
  margin: 20px auto;
}

.MovieDetail h2 {
  margin-top: 0;
}

.MovieDetail span {
  font-weight: bold;
}

.MovieDetail img {
  max-width: 100%;
  border-radius: 8px;
  margin-bottom: 20px;
}

.MovieDetail p {
  line-height: 1.5;
}

.close {
  background-color: #333;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.close:hover {
  background-color: #555;
}

.search-bar {
  margin: 20px auto;
  max-width: 400px;
}

.search {
  width: 100%;
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ddd;
  border-radius: 4px;
  box-sizing: border-box;
  transition: border-color 0.3s ease;
}

.search:focus {
  outline: none;
  border-color: #333;
}
</style>