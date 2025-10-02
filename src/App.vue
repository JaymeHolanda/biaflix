<template>
  <div :class="{ 'dark-mode': isDarkMode }">
    <nav class="navbar navbar-expand-lg bg-transparent border-bottom">
      <div class="container-fluid">
        <router-link to="/">
          <a class="navbar-brand">
            <img src="./assets/images/biaflix.png" class="img" alt="Logo Biaflix" />
          </a>
        </router-link>
        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <ul class="navbar-nav me-auto mb-2 mb-lg-0">
            <li class="nav-item">
              <router-link to="/" class="nav-link">Home</router-link>
            </li>
            <li class="nav-item">
              <router-link to="/series" class="nav-link">Séries</router-link>
            </li>
            <li class="nav-item">
              <router-link to="/filmes" class="nav-link">Filmes</router-link>
            </li>
            <li class="nav-item dropdown">
              <a class="nav-link dropdown-toggle" href="#" role="button" style="color: #212121"
                data-bs-toggle="dropdown" aria-expanded="false">
                Categorias
              </a>
              <ul class="dropdown-menu">
                <li>
                  <router-link to="/action" class="dropdown-item">Ação</router-link>
                </li>
                <li>
                  <router-link to="/adventure" class="dropdown-item">Aventura</router-link>
                </li>
                <li>
                  <router-link to="/comedy" class="dropdown-item">Comédia</router-link>
                </li>
                <li>
                  <router-link to="/romantic-comedy" class="dropdown-item">Comédia Romântica</router-link>
                </li>
                <li>
                  <router-link to="/drama" class="dropdown-item">Drama</router-link>
                </li>
                <li>
                  <router-link to="/fantasy" class="dropdown-item">Fantasia</router-link>
                </li>
                <li>
                  <router-link to="/sci-fi" class="dropdown-item">Ficção Científica</router-link>
                </li>
                <li>
                  <router-link to="/musical" class="dropdown-item">Musical</router-link>
                </li>
                <li>
                  <router-link to="/romance" class="dropdown-item">Romance</router-link>
                </li>
                <li>
                  <router-link to="/horror" class="dropdown-item">Terror</router-link>
                </li>
                <li>
                  <hr class="dropdown-divider" />
                </li>
                <li>
                  <router-link to="/all" class="dropdown-item">Todos</router-link>
                </li>
              </ul>
            </li>
          </ul>
          <form class="d-flex" role="search" @submit.prevent="searchMovies">
            <input class="form-control me-2 bg-transparent" type="search" placeholder="Buscar" aria-label="Buscar"
              v-model="searchQuery" />
          </form>
          <div class="dark-mode-toggle ms-3" @click="toggleDarkMode">
            <svg xmlns="http://www.w3.org/2000/svg" height="24" viewBox="0 0 24 24" width="24" class="dark-mode-icon">
              <path d="M0 0h24v24H0z" fill="none"/>
              <path d="M12 3c-4.97 0-9 4.03-9 9s4.03 9 9 9 9-4.03 9-9c0-.46-.04-.92-.1-1.36-.98 1.37-2.58 2.26-4.4 2.26-2.98 0-5.4-2.42-5.4-5.4 0-1.81.89-3.42 2.26-4.4-.44-.06-.9-.1-1.36-.1z"/>
            </svg>
          </div>
        </div>
      </div>
    </nav>

    <section class="body">
      <div v-if="searchResults.length > 0" class="container">
        <h2>Resultados da Busca</h2>
        <ul class="cards">
          <li v-for="movie in searchResults" :key="movie.id">
            <a :href="movie.movieURL" target="_blank">
              <img class="image" :src="movie.imageURL" :alt="movie.name">
            </a>
          </li>
        </ul>
        <div class="body">
          <section class="container">
            <h2>Outras opções</h2>
            <div class="cards">
              <li v-for="(movie, index) in movies" :key="index">
                <div>
                  <a :href="movie.movieURL" target="_blank">
                    <img class="image" :src="movie.imageURL" :alt="movie.name">
                  </a>
                </div>
              </li>
            </div>
          </section>
        </div>
      </div>
      <div v-else-if="searchQuery && searchResults.length === 0">
        <h3>"{{ searchQuery }}" não encontrado</h3>
        <div class="body">
          <section class="container">
            <h2>Outras opções</h2>
            <div class="cards">
              <li v-for="(movie, index) in movies" :key="index">
                <div>
                  <a :href="movie.movieURL" target="_blank">
                    <img class="image" :src="movie.imageURL" :alt="movie.name">
                  </a>
                </div>
              </li>
            </div>
          </section>
        </div>
      </div>
      <router-view v-if="!searchQuery" />

    </section>

    <footer class=" footer">
      <div class="logo">
        <a href="#">
          <img src="./assets/images/biaflix.png" class="img" alt="Logo Biaflix" />
        </a>
      </div>
      <div class="icons">
        <img src="./assets/icons/github.svg" alt="Ícone do Github" />
        <img src="./assets/icons/instagram.svg" alt="Ícone do Instagram" />
        <img src="./assets/icons/linkedin.svg" alt="Ícone do LinkedIn" />
      </div>
      <div class="text">Copyright © 2024</div>
    </footer>
  </div>
</template>

<script>
import list from './data/data.json'
export default {
  name: "App",
  data() {
    return {
      movies: list.movies,
      searchQuery: '',
      searchResults: [],
      isDarkMode: false
    };
  },
  methods: {
    searchMovies() {
      if (this.searchQuery) {
        this.searchResults = this.movies.filter(movie =>
          movie.name.toLowerCase().includes(this.searchQuery.toLowerCase())
        );
      } else {
        this.searchResults = [];
      }
    },
    toggleDarkMode() {
      this.isDarkMode = !this.isDarkMode;
    }
  },
  watch: {
    searchQuery() {
      this.searchMovies();
    }
  }
};

</script>

<style scoped>
/* Light mode (default) styles */
.navbar {
  padding: 10px 30px;
}

.nav-link {
  color: #212121;
}

.img {
  width: 100%;
  max-width: 60px;
  margin-bottom: 25px;
}

.footer {
  text-align: center;
  padding: 30px;
  background-color: #6c757dd8;
  box-shadow: 0 -10px 20px rgba(108, 117, 125, 0.847);
  position: relative;
}

.icons {
  padding-bottom: 20px;
}

.icons img {
  margin: 5px;
}

/* Dark mode toggle button */
.dark-mode-toggle {
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 5px;
  border-radius: 50%;
  transition: background-color 0.3s ease;
}

.dark-mode-toggle:hover {
  background-color: rgba(0, 0, 0, 0.1);
}

.dark-mode-icon {
  fill: #212121;
  transition: fill 0.3s ease;
}

/* Dark mode styles */
.dark-mode {
  background-color: #121212;
  color: #f5f5f5;
}

.dark-mode .navbar {
  border-bottom-color: #333 !important;
}

.dark-mode .nav-link,
.dark-mode a,
.dark-mode h1,
.dark-mode h2,
.dark-mode h3 {
  color:rgb(255, 255, 255);
}

.dark-mode #navbar {
  border-bottom-color: #333 !important;
}


.dark-mode .dropdown-menu {
  background-color: #333;
}

.dark-mode .dropdown-item {
  color: #f5f5f5;
}

.dark-mode .dropdown-item:hover {
  background-color: #444;
}

.dark-mode .form-control {
  background-color: rgba(255, 255, 255, 0.1) !important;
  color: #f5f5f5;
  border-color: #444;
}

.dark-mode .footer {
  background-color: #333;
  box-shadow: 0 -10px 20px rgba(0, 0, 0, 0.3);
}

.dark-mode .dark-mode-icon {
  fill: #f5f5f5;
}

.dark-mode .dropdown-toggle {
  color: #f5f5f5 !important;
}

.dark-mode .dropdown-divider {
  border-top-color: #444;
}
</style>
