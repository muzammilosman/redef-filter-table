<template>
  <div id="app">
    <table-filter />
  </div>
</template>

<script>
import TableFilter from "./components/filter.vue";

export default {
  name: "App",
  components: {
    TableFilter
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.md-menu-content-container{
  background: white !important;
}
</style>
