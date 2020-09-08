<template>
  <div id="app">
    <input type="text" v-model="lang" />

    <input type="text" v-model="startDate" />

    <button @click="id++">refresh</button>

    <hr />

    <Calendar :lang="lang" :start-date="startDate" :key="id" @select="select" />
  </div>
</template>

<script>
import Calendar from "./components/Calendar.vue";

export default {
  name: "App",
  components: {
    Calendar,
  },
  data() {
    return {
      lang: "en",
      startDate: "",
      id: 0,
    };
  },
  methods: {
    select(par) {
      console.log("Parent component: ", par);
    },
  },
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
</style>
