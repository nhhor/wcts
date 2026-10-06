<script setup lang="ts">
import HeaderComp from "./components/HeaderComp.vue";
import BodyComp from "./components/BodyComp.vue";
import FooterComp from "./components/FooterComp.vue";
</script>

<template>
  <div id="app">
    <HeaderComp msg="What Can They See?" />
    <BodyComp />
    <FooterComp />
  </div>
</template>

<style scoped>
#app {
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: #c9c9ce;
}
</style>
