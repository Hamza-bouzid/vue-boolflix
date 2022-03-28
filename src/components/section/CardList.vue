<template>
  <div class="cardList">
    <div class="heading">
      <div class="logo">
        <img src="../../assets/img/boolflix.png" alt="Boolflix" />
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
          <h3>Risultato Ricerca | <span>{{ search }}</span></h3>
        </div>
        <div class="cards">
          <Card v-for="(element, index) in listaFilm" :key="index" :card="element" />
        </div>

        <div v-if="listaFilm.length >= 1" class="nextPrev">
          <button :class="{ hidden: pageFilm == 1 }" @click="prevPage('film')"><i class="fas fa-chevron-left"></i> {{ pageFilm - 1 }}</button>
          <span>{{ pageFilm }}</span>
          <button :class="{ hidden: pageFilm == totalPagesFilm }" @click="nextPage('film')">{{ pageFilm + 1 }} <i class="fas fa-chevron-right"></i></button>
        </div>
      </div>

      <div v-else-if="!showTv" class="film">
        <div class="type">
          <h2>I Film del Momento</h2>
        </div>
        <div class="cards">
          <Card v-for="(element, index) in discoveredMovie" :key="index" :card="element" />
        </div>
      </div>

      <div v-if="showTv" class="serieTV">
        <div class="type">
          <h2>Serie Tv</h2>
          <h3>Risultato Ricerca | <span>{{ search }}</span></h3>
        </div>
        <div class="cards">
          <Card v-for="(elemento, indice) in listaTv" :key="indice" :card="elemento" />
        </div>

        <div v-if="listaTv.length >= 1" class="nextPrev">
          <button :class="{ hidden: pageTv == 1 }" @click="prevPage('tv')"><i class="fas fa-chevron-left"></i> {{ pageTv - 1 }}</button>
          <span>{{ pageTv }}</span>
          <button :class="{ hidden: pageTv == totalPagesTv }" @click="nextPage('tv')">{{ pageTv + 1 }} <i class="fas fa-chevron-right"></i></button>
        </div>
      </div>

      <div v-else-if="!showFilm" class="serieTV">
        <div class="type">
          <h2>Le Serie del Momento</h2>
        </div>
        <div class="cards">
          <Card v-for="(element, index) in discoveredTv" :key="index" :card="element" />
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
      discoveredMovie: [],
      discoveredTv: [],
      showFilm: false,
      showTv: false,
      search: "",
      type: "",
      pageTv: 1,
      pageFilm: 1,
      totalPagesFilm: null,
      totalPagesTv: null,
    };
  },

  created() {
    axios
      .get("https://api.themoviedb.org/3/discover/movie?", {
        params: {
          api_key: "a98e27fc86b1afd6d8445aaa1cefc645",
          include_adult: "false",
        },
      })
      .then((risposta) => {
        this.discoveredMovie = risposta.data.results;
        console.log(this.discoveredMovie);
      });

    axios
      .get("https://api.themoviedb.org/3/discover/tv?", {
        params: {
          api_key: "a98e27fc86b1afd6d8445aaa1cefc645",
          include_adult: "false",
        },
      })
      .then((risposta) => {
        this.discoveredTv = risposta.data.results;
        console.log(this.discoveredTv);
      });
  },

  methods: {
    getCardMovies: function () {
      axios
        .get("https://api.themoviedb.org/3/search/movie", {
          params: {
            api_key: "a98e27fc86b1afd6d8445aaa1cefc645",
            query: this.search,
            include_adult: "false",
            page: this.pageFilm,
          },
        })
        .then((risposta) => {
          this.listaFilm = risposta.data.results;
          console.log(this.listaFilm);
          this.totalPagesFilm = risposta.data.total_pages;
          console.log(this.totalPagesFilm);
        });
    },
    getCardTv: function () {
      axios
        .get("https://api.themoviedb.org/3/search/tv", {
          params: {
            api_key: "a98e27fc86b1afd6d8445aaa1cefc645",
            query: this.search,
            include_adult: "false",
            page: this.pageTv,
          },
        })
        .then((risposta) => {
          this.listaTv = risposta.data.results;
          console.log(this.listaTv);
          this.totalPagesTv = risposta.data.total_pages;
          console.log(this.totalPagesTv);
        });
    },

    selectTypeToWatch: function (inputSelect) {
      this.type = inputSelect;
      console.log(this.type);
    },

    searchByName: function (input) {
      this.search = input;
      this.pageTv = 1;
      this.pageFilm = 1;
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

    nextPage: function (tipo) {
      if (tipo == "film") {
        this.pageFilm++;
        this.getCardMovies();
      } else if (tipo == "tv") {
        this.pageTv++;
        this.getCardTv();
      }
    },
    prevPage: function (tipo) {
      if (this.pageFilm >= 2 || this.pageTv >= 2) {
        if (tipo == "film") {
          this.pageFilm--;
          this.getCardMovies();
        } else if (tipo == "tv") {
          this.pageTv--;
          this.getCardTv();
        }
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.conatiner {
  max-width: 1200px;
  margin: 0 auto;

  h3 {
    color: #808080;
    font-size: 20px;

    span {
      color: #f0eaea;
    }
  }
}

.cards {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
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
}

.serieTv,
.film {
  margin-bottom: 60px;
}

.type {
  text-align: center;
  font-size: 40px;
}

.nextPrev {
  display: flex;
  justify-content: center;
  align-items: center;
  button {
    background-color: #b6141a;
    color: #fff;
    border: none;
    padding: 5px 10px;
    cursor: pointer;
    margin: 0px 10px;
  }

  i {
    margin: 0px 3px;
  }
}

.hidden {
  visibility: hidden;
}
</style>
