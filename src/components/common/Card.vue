<template>
  <div class="card">
    <!--Card Front-->
    <div class="card-front">
      <img v-if="card.poster_path" :src="image + card.poster_path" />
      <img v-else-if="card.backdrop_path" :src="image + card.backdrop_path" />
      <!-- <img v-else-if="card.profile_path" :src="image + card.profile_path" /> -->
      <img v-else src="../../assets/img/no-picture.webp" />
    </div>
    <!--Card Back-->
    <div class="card-back">
      <ul>
        <li><span>Titolo:</span> {{ card.title }} {{ card.name }}</li>
        <li><span>Titolo originale:</span>{{ card.original_title }} {{ card.original_name }}</li>
        <li><span>Lingua:</span><img :src="getFlag(card.original_language)" /></li>
        <li>
          <!--{{ arrotondareNumero(card.vote_average) }}-->
          <span>Voto:</span>
          <div class="stars"><div class="star-inner" :class="starsWidth(arrotondareNumero(card.vote_average))"></div></div>
        </li>
      </ul>
      <div class="overview-text"><span>Overview:</span>{{ card.overview }}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Card",

  data() {
    return {
      image: "https://image.tmdb.org/t/p/w342",
      flagImage: "https://flagcdn.com/24x18/",
    };
  },

  methods: {
    arrotondareNumero: function (voto) {
      return Math.ceil(voto / 2);
    },

    getFlag: function (flag) {
      if (flag == "en") {
        return this.flagImage + "gb.png";
      } else if (flag == "it") {
        return this.flagImage + "it.png";
      } else {
        return this.flagImage + "aq.png";
      }
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
  height: 300px;
  margin: 30px 20px;
  cursor: pointer;
  position: relative;

  &-front {
    img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }
  }

  &-front,
  &-back {
    width: 100%;
    height: 100%;
    overflow: hidden;
    backface-visibility: hidden;
    position: absolute;
    transition: all 0.6s linear;
    border-radius: 10px;
  }

  &-front {
    transform: perspective(600px) rotateY(0deg);
  }

  &-back {
    padding: 10px;
    background-color: rgba(0, 0, 0, 0.9);
    transform: perspective(600px) rotateY(180deg);
  }

  &:hover > .card-front {
    transform: perspective(600px) rotateY(-180deg);
  }
  &:hover > .card-back {
    transform: perspective(600px) rotateY(0deg);
  }

  span {
    font-weight: bold;
    margin-right: 5px;
    color: #fff;
    font-size: 15px;
  }

  li {
    margin: 5px 0px;
    img {
      width: 20px;
    }
  }
}

.overview-text {
  max-height: 150px;
  font-size: 12px;
  text-overflow: ellipsis;
  overflow: hidden;
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
