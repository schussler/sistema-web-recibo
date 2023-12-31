<template>
  <div class="home">
    <NavBar />
    <img alt="Vue logo" src="../assets/logo.png" />
  </div>
</template>

<script>
// @ is an alias to /src
import NavBar from "@/components/NavBarComponents.vue";

export default {
  name: "HomeView",
  components: {
    NavBar,
  },
};
</script>
