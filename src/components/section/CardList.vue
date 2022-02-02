<template>
  <div class="cardList">
    <Search @cerca="searchByName" />
    <div class="conatiner">
      <div class="cards">
        <Card v-for="(element, index) in lista" :key="index" :card="element" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Card from "../common/Card.vue";
import Search from "../common/Search.vue";
export default {
  name: "CardList",

  components: {
    Card,
    Search,
  },

  data() {
    return {
      lista: [],
      search: "",
    };
  },

  methods: {
    getCard: function () {
      axios
        .get("https://api.themoviedb.org/3/search/multi?", {
          params: {
            api_key: "a98e27fc86b1afd6d8445aaa1cefc645",
            query: this.search,
            include_adult: "false",
          },
        })
        .then((risposta) => {
          this.lista = risposta.data.results;
          console.log(this.lista);
        });
    },

    searchByName: function (input) {
      this.search = input;
      this.getCard();
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
}
</style>
