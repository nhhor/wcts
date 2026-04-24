<script setup lang="ts">
// import { ref } from 'vue' 
import MousePosition from "./MousePosition.vue";

import MouseIcon from "./icons/IconMouse.vue";


const items = [
	{
		src: 'https://assets.codepen.io/2017/17_05_a_amur_leopard_14.jpg', 
		alt: 'fluffy, alert Amur leopard',
	}, 
	{
    alt: 'fluffy Amur leopard looking up',
    icon: MouseIcon,
    header: 'mouse',
    description: '... where your mouse is on the tab.',
    child: MousePosition
	}, 
	{
		src: 'https://assets.codepen.io/2017/17_05_a_amur_leopard_16.jpg', 
		alt: 'fluffy Amur leopard on a tree branch'
	}, 
	{
		src: 'https://assets.codepen.io/2017/17_05_a_amur_leopard_18.jpg', 
		alt: 'Amur leopard romance'
	}, 
	{
		src: 'https://assets.codepen.io/2017/17_05_a_amur_leopard_24.jpg', 
		alt: 'Amur leopard cub with very blue eyes'
	}, 
	{
		src: 'https://assets.codepen.io/2017/17_05_a_amur_leopard_30.jpg', 
		alt: 'fluffy, alert Amur leopard'
	}
];

let n_items = items.length;
let has_mid = 1; /* 0 if there's no item in the middle, 1 otherwise */
let m = n_items - has_mid; /* how many are ON the circle */

</script>

<template>
  <div class="container" :style="{ '--m': m }">
    <div v-for="(item, index) in items" :style="index - has_mid >= 0 ? `--i: ${index}` : null" :key="index">
      <span>
        ({{ index > 0 ? index : '' }})
        <img v-if="item && item.src && item.src.length > 0" :src="item.src" :alt="item.alt">
        <component v-if="item.icon" :is="item.icon" />
        <!-- <h3 v-if="item.header">{{ item.header }}</h3> -->
        <!-- <p v-if="item.description">{{ item.description }}</p> -->

        <component v-if="item.child" :is="item.child" />
      </span>
    </div>
  </div>
</template>

<style scoped>
div { aspect-ratio: 1 }

.container {
  --d: 6.5em; /* image size */
	--rel: 1; /* how much extra space we want between images, 1 = 1 image size */
	--ba: 1turn/var(--m);
  --r: calc(.5*(1 + var(--rel))*var(--d)/tan(.5*var(--ba))); /* circle radius */
  display: grid;
  width: calc(2*var(--r) + var(--d)); /* container size */
	background: silver
}

.container div {
	grid-area: 1/ 1;
  place-self: center;
	width: var(--d);
  --ca: calc(var(--i)*var(--ba));
  transform: 
    rotate(var(--ca)) 
    translate(var(--r))
    rotate(calc(-1*var(--ca)))
}

img { max-width: 100% }
</style>
