<template>
  <div id="app">
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
</style>
