<template>
  <div class="cardList">
    <div class="heading">
      <div class="logo">
        <h1>Boolflix</h1>
      </div>
      <div class="search">
        <SelectType @select="selectTypeToWatch" />
        <Search @cerca="searchByName" />
      </div>
    </div>

    <div class="conatiner">
      <div v-if="showFilm" class="film">
        <div class="type">
          <h2>Film</h2>
        </div>
        <div class="cards">
          <Card v-for="(element, index) in listaFilm" :key="index" :card="element" />
        </div>
      </div>
      <div v-if="showTv" class="serieTV">
        <div class="type">
          <h2>Serie Tv</h2>
        </div>
        <div class="cards">
          <Card v-for="(elemento, indice) in listaTv" :key="indice" :card="elemento" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Card from "../common/Card.vue";
import Search from "../common/Search.vue";
import SelectType from "../common/SelectType.vue";
export default {
  name: "CardList",

  components: {
    Card,
    Search,
    SelectType,
  },

  data() {
    return {
      listaFilm: [],
      listaTv: [],
      showFilm: false,
      showTv: false,
      search: "",
      type: "",
    };
  },

  methods: {
    getCardMovies: function () {
      axios
        .get("https://api.themoviedb.org/3/search/movie", {
          params: {
            api_key: "a98e27fc86b1afd6d8445aaa1cefc645",
            query: this.search,
            include_adult: "false",
          },
        })
        .then((risposta) => {
          this.listaFilm = risposta.data.results;
          console.log(this.listaFilm);
        });
    },
    getCardTv: function () {
      axios
        .get("https://api.themoviedb.org/3/search/tv", {
          params: {
            api_key: "a98e27fc86b1afd6d8445aaa1cefc645",
            query: this.search,
            include_adult: "false",
          },
        })
        .then((risposta) => {
          this.listaTv = risposta.data.results;
          console.log(this.listaTv);
        });
    },

    selectTypeToWatch: function (inputSelect) {
      this.type = inputSelect;
      console.log(this.type);
    },

    searchByName: function (input) {
      this.search = input;
      if (this.type == "film") {
        this.getCardMovies();
        this.showFilm = true;
        this.showTv = false;
      } else if (this.type == "serietv") {
        this.getCardTv();
        this.showFilm = false;
        this.showTv = true;
      } else if (this.type == "filmserie") {
        this.getCardMovies();
        this.getCardTv();
        this.showFilm = true;
        this.showTv = true;
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.conatiner {
  max-width: 1200px;
  margin: 0 auto;
}
.cards {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
  margin-bottom: 50px;
}

.search {
  display: flex;
}

.heading {
  background-color: #000;
  padding: 20px;
  display: flex;
  justify-content: space-around;
  align-items: center;
  margin-bottom: 60px;

  .logo {
    h1 {
      font-size: 50px;
      color: #b6141a;
      text-transform: uppercase;
    }
  }
}

.type {
  text-align: center;
  font-size: 40px;
}
</style>
