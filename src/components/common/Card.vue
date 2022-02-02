<template>
  <div class="card">
    <ul>
      <div class="card-image">
        <img v-if="card.poster_path" :src="image + card.poster_path" />
        <img v-else-if="card.known_for[0].poster_path" :src="image + card.known_for[0].poster_path" />
        <img v-else src="../../assets/img/no-picture.webp" />
      </div>

      <li><span></span>{{ card.title }} {{ card.name }}</li>

      <li v-if="card.original_title || card.original_name">{{ card.original_title }} {{ card.original_name }}</li>
      <li v-else>{{ card.known_for[0].original_name }}</li>

      <li v-if="card.original_language == 'it'"><img src="../../assets/img/italia.png" /></li>
      <li v-else-if="card.original_language == 'en'"><img src="../../assets/img/england.png" /></li>
      <li v-else-if="card.original_language != 'en' || card.original_language != 'it'"><img src="../../assets/img/globe.jpg" /></li>

      <li v-if="card.vote_average">{{ arrotondareNumero(card.vote_average) }}</li>
      <li v-else>{{arrotondareNumero(card.known_for[0].vote_average)}}</li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Card",

  data() {
    return {
      image: "https://image.tmdb.org/t/p/w500",
    };
  },

  methods: {
    arrotondareNumero: function (voto) {
      return Math.ceil(voto / 2);
    },
  },
  props: {
    card: Object,
  },
};
</script>

<style lang="scss" scoped>
li {
  list-style: none;
}
.card {
  width: 300px;
  height: 500px;

  &-image {
    width: 100%;
    height: 400px;
    img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }
  }

  li {
    img {
      width: 20px;
    }
  }
}
</style>
