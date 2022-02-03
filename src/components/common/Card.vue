<template>
  <div class="card">
    <ul>
      <div class="card-image">
        <img v-if="card.poster_path" :src="image + card.poster_path" />
        <img v-else-if="card.backdrop_path" :src="image + card.backdrop_path" />
        <img v-else-if="card.profile_path" :src="image + card.profile_path" />
        <img v-else src="../../assets/img/no-picture.webp" />
      </div>

      <li>{{ card.title }} {{ card.name }}</li>

      <li v-if="card.original_title || card.original_name">{{ card.original_title }} {{ card.original_name }}</li>

      <li v-if="card.original_language == 'it'"><img :src="flag + 'it.png'" /></li>
      <li v-else-if="card.original_language == 'en'"><img :src="flag + 'gb.png'" /></li>
      <li v-else><img :src="flag + 'aq.png'" /></li>

      <li>
        {{ arrotondareNumero(card.vote_average) }}
        <div class="stars"><div class="star-inner" :class="starsWidth(arrotondareNumero(card.vote_average))"></div></div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Card",

  data() {
    return {
      image: "https://image.tmdb.org/t/p/w500",
      flag: "https://flagcdn.com/24x18/",
    };
  },

  methods: {
    arrotondareNumero: function (voto) {
      return Math.ceil(voto / 2);
    },

    starsWidth: function (numero) {
      return "starFill" + numero;
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
  width: 200px;
  height: 200px;
  margin: 100px 10px;

  &-image {
    width: 100%;
    height: 300px;
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

  .stars {
    position: relative;
    display: inline-block;

    &::before {
      content: "\f005 \f005 \f005 \f005 \f005";
      font-family: "Font Awesome 5 Free";
      font-weight: 900;
      color: rgba(204, 204, 204, 0.507);
    }
  }

  .star-inner {
    position: absolute;
    top: 0;
    left: 0;
    white-space: nowrap;
    overflow: hidden;

    &::before {
      content: "\f005 \f005 \f005 \f005 \f005";
      font-family: "Font Awesome 5 Free";
      font-weight: 900;
      color: #f8ce0b;
    }
  }
}

.starFill0 {
    width: 0%;
  }
  .starFill1 {
    width: 20%;
  }
  .starFill2 {
    width: 40%;
  }
  .starFill3 {
    width: 60%;
  }
  .starFill4 {
    width: 80%;
  }
  .starFill5 {
    width: 100%;
  }
</style>
