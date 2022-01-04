<template>
  <div id="app">
    <chat-app msg="Welcome to Your Vue.js App" />
  </div>
</template>

<script>
import ChatApp from "./components/ChatApp.vue";

export default {
  name: "App",
  components: {
    ChatApp,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
