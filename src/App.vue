<template>
  <HomeComponent />
</template>

<script>
import HomeComponent from "./components/home/HomeComponent.vue";

export default {
  name: "App",
  components: {
    HomeComponent,
  },
};
</script>

<style>
#app {
  font-family:Arial, Helvetica, sans-serif;
  text-align: center;
}
</style>
