<template>
  <div id="app">
    <Main />
  </div>
</template>

<script>
import Main from "./components/Main.vue";
export default {
  name: "App",
  components: {
    Main,
  },
};
</script>

<style lang="scss">
@import url("./assets/style/global.scss");

#app {
  background: rgba(0, 0, 0, 0.5);
  border-radius: 16px;
  box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
  backdrop-filter: blur(6.4px);
  -webkit-backdrop-filter: blur(6.4px);
}
</style>
