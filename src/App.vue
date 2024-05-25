<template>
  <main>
    <h1>Minha lista de animes</h1>
    <SearchForm
      @search="searchAnime"
      @update:query="updateQuery"
      :query="query"
    />
    <SearchResults
      v-if="search_results.length > 0"
      :results="search_results"
      @add="addAnime"
    />
    <MyAnimeList
      :animes="my_anime_asc"
      @increase="increaseWatch"
      @decrease="decreaseWatch"
      @remove="removeAnime"
    />
  </main>
</template>

<script>
import Swal from "sweetalert2";
import SearchForm from "./components/SearchForm.vue";
import SearchResults from "./components/SearchResults.vue";
import MyAnimeList from "./components/MyAnimeList.vue";

export default {
  components: {
    SearchForm,
    SearchResults,
    MyAnimeList,
  },

  data() {
    return {
      query: "",
      my_anime: [],
      search_results: [],
    };
  },

  mounted() {
    this.my_anime = JSON.parse(localStorage.getItem("my_anime")) || [];
  },

  computed: {
    my_anime_asc() {
      return this.my_anime.sort((a, b) => a.title.localeCompare(b.title));
    },
  },

  methods: {
    searchAnime() {
      const url = `https://api.jikan.moe/v4/anime?q=${this.query}`;
      fetch(url)
        .then((res) => res.json())
        .then((data) => {
          this.search_results = data.data;
          console.log(this.search_results)
        });
    },

    // handleInput(e) {
    //   if (!e.target.value) {
    //     this.search_results = [];
    //   }
    // },

    updateQuery(newQuery) {
      this.query = newQuery;
    },

    addAnime(anime) {
      console.log(anime)
      if (this.my_anime.some((a) => a.id === anime.mal_id)) {
        Swal.fire({
          position: "bottom-end",
          icon: "error",
          title: "Não é possível adicionar o mesmo anime",
          showConfirmButton: false,
          timer: 1500,
          toast: true,
        });
        return;
      }

      this.search_results = [];
      this.query = "";

      this.my_anime.push({
        id: anime.mal_id,
        title: anime.title,
        image: anime.images.jpg.image_url,
        total_episodes: anime.episodes,
        watched_episodes: 0,
      });

      localStorage.setItem("my_anime", JSON.stringify(this.my_anime));

      Swal.fire({
        position: "bottom-end",
        icon: "success",
        title: "Novo anime adicionado na lista",
        showConfirmButton: false,
        timer: 1500,
        toast: true,
      });
    },

    increaseWatch(anime) {
      anime.watched_episodes++;
      localStorage.setItem("my_anime", JSON.stringify(this.my_anime));

      if (anime.watched_episodes === anime.total_episodes) {
        Swal.fire({
          position: "bottom-end",
          icon: "success",
          title: "Mais um anime finalizado!",
          showConfirmButton: false,
          timer: 1500,
          toast: true,
        });
      }
    },

    decreaseWatch(anime) {
      anime.watched_episodes--;
      localStorage.setItem("my_anime", JSON.stringify(this.my_anime));
    },

    removeAnime(anime) {
      this.my_anime = this.my_anime.filter((a) => a.id !== anime.id);
      localStorage.setItem("my_anime", JSON.stringify(this.my_anime));

      Swal.fire({
        position: "bottom-end",
        icon: "error",
        title: "Mais um anime dropado :(",
        showConfirmButton: false,
        timer: 1500,
        toast: true,
      });
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Fira Sans", sans-serif;
}

body {
  background-color: #eee;
}

main {
  margin: 0 auto;
  max-width: 768px;
  padding: 1.5rem;
}

h1 {
  text-align: center;
  margin-bottom: 1.5rem;
}
</style>