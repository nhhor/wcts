<script setup lang="ts">
import TheAllSeeingEye from "./TheAllSeeingEye.vue";
import MousePosition from "./MousePosition.vue";
import MousePressed from "./MousePressed.vue";
import Permissions from "./Permissions.vue";

const items = [
	{
		child: TheAllSeeingEye,
		itemProps: {
			title: 'The All-Seeing Eye',
		}
	}, 
	{
		child: MousePosition,
		itemProps: {
			title: 'Mouse Position',
			tooltip: 'They can see where your mouse is...'
		}
	}, 
	{
		child: MousePressed,
		itemProps: {
			title: 'Mouse Pressed',
			tooltip: 'They can see if your mouse is pressed...'
		}
	}, 
	{
		child: Permissions,
		itemProps: {
			title: 'Permissions',
			tooltip: 'They can see additional permissions... (CLICK MENU)'
		}
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
		<img v-if="item.src && item.src.length > 0" :src="item.src" :alt="item.alt">
		<component v-if="item.child" :is="item.child" v-bind="item.itemProps" :index="index"/>
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
	margin: 0 auto;
	border-radius: 50%;
	background: radial-gradient(circle at center, rgba(0, 0, 0, 0) 0%, rgba(0, 255, 255, .1) 60%, rgba(0, 0, 0, 0) 70%);
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
