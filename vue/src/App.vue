<template>
  <div id="app">
    <header class="app-header">
      <h1>Deaths in NYC</h1>
    </header>

    <Home />
  </div>
</template>

<script>
import Home from "./components/Home.vue";

export default {
  components: {
    Home,
  },
};
</script>

<style>
html,
body {
  height: 100%;
  margin: 0;
  padding: 0;
}

#app {
  height: 100%;
  display: flex;
  flex-direction: column;
}
#app {
  width: 100%;
}

.home-container {
  width: 100%;
}

.app-header {
  color: black;
  text-align: center;
  padding: 20px 0;
  font-size: 24px;
}

.home-container {
  flex-grow: 1;
  width: 100%;
}
</style>
