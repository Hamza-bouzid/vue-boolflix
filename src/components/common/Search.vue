<template>
  <div class="search">
    <input type="text" v-model.trim="searchInput" />
    <button @click="getInput">Cerca</button>
  </div>
</template>

<script>
export default {
  name: "Search",

  data() {
    return {
      searchInput: "",
    };
  },

  methods: {
    getInput: function () {
      this.$emit("cerca", this.searchInput);
      this.searchInput = "";
    },
  },
};
</script>

<style lang="scss" scoped>

.search {
  input {
    padding: 5px 20px;
    background-color: rgba(255, 255, 255, 0.13);
    color: #fff;
    border: 1px solid #fff;
    outline: none;
  }

  button {
    padding: 5px 10px;
     border: 1px solid #b6141a;
     background-color: #b6141a;
     color: #fff;
     cursor: pointer;
  }
}

</style>
