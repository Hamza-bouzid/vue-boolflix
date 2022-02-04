<template>
  <div class="select">
    <select name="selectType" @change="valueSelect">
      <option>--</option>
      <option value="filmserie">Film/Serie TV</option>
      <option value="film">Film</option>
      <option value="serietv">Serie Tv</option>
    </select>
  </div>
</template>

<script>
export default {
  name: "SelectType",

  data() {
    return {
      selectedValue: "",
    };
  },

  methods: {
    valueSelect: function (e) {
      this.valueSelect = e.target.value;
      this.$emit("select", this.valueSelect);
    },
  },
};
</script>

<style lang="scss" scoped></style>
