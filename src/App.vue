<script setup lang="ts">
import HeaderComp from "./components/HeaderComp.vue";
import MainComp from "./components/MainComp.vue";
import FooterComp from "./components/FooterComp.vue";
</script>

<template>
  <header>
    <HeaderComp msg="What Can They See?" />
  </header>
  <main>
    <MainComp />
  </main>
  <footer>
    <FooterComp />
  </footer>
</template>

<style scoped>

#app {
  width: 100vw;
  /* display: flex; */
  /* flex-direction: column; */
  /* min-height: 100vh; */
}

header {
  line-height: 1.5;
}

footer {
  text-align: center;
  padding: 2rem 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }
}
</style>
