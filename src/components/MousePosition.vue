<script setup lang="ts">
import { useMouse } from '@vueuse/core'
const { x, y } = useMouse()
</script>

<template>
    <p>[{{ x }}, {{ y }}]</p>
</template>

<style scoped>
.p {
  text-align: center;
  position: fixed;
  bottom: 1rem;
  right: 1rem;
  background: rgba(255, 255, 255, 0.8);
  padding: 0.5rem;
  border-radius: 0.25rem;
  font-size: 0.9rem;
  color: red;
}
</style>
