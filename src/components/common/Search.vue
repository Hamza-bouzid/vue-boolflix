<template>
  <div class="search">
    <input type="text" v-model="searchInput" />
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

<style></style>
