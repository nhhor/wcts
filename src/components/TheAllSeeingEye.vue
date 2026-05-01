<script setup lang="ts">
import { computed, useTemplateRef, reactive } from 'vue'
import type { CSSProperties } from 'vue'
import { useParallax, useInterval } from '@vueuse/core'

const counter = useInterval(200)
const blinkCounter = computed(() => counter.value % 40 === 0)

const target = useTemplateRef('target')
const parallax = reactive(useParallax(target))

const targetStyle: CSSProperties = {
  display: 'flex',
  flexDirection: 'column',
  justifyContent: 'center',
  minHeight: '500px',
  transition: '.3s ease-out all',
}
const cardWindowStyle: CSSProperties = {
  overflow: 'hidden',
  fontSize: '6rem',
  position: 'absolute',
  top: 'calc(50% - 1em)',
  left: 'calc(50% - 1em)',
  height: '2em',
  width: '2em',
  margin: 'auto',
}
// const layerBase: CSSProperties = {
//   position: 'absolute',
//   height: '100%',
//   width: '100%',
//   transition: '.3s ease-out all',
// }
const containerStyle: CSSProperties = {
  margin: '3em auto',
  perspective: '300px',
}

// const layer0 = computed(() => ({
//   ...layerBase,
//   transform: `translateX(${parallax.tilt * 10}px) translateY(${
//     parallax.roll * 10
//   }px) scale(1.33)`,
// }))

// const layer1 = computed(() => ({
//   ...layerBase,
//   transform: `translateX(${parallax.tilt * 20}px) translateY(${
//     parallax.roll * 20
//   }px) scale(1.33)`,
// }))

// const layer2 = computed(() => ({
//   ...layerBase,
//   transform: `translateX(${parallax.tilt * 30}px) translateY(${
//     parallax.roll * 30
//   }px) scale(1.33)`,
// }))

// const layer3 = computed(() => ({
//   ...layerBase,
//   transform: `translateX(${parallax.tilt * 40}px) translateY(${
//     parallax.roll * 40
//   }px) scale(1.33)`,
// }))

// const layer4 = layerBase

const cardStyle = computed(() => ({
  background: 'radial-gradient(circle at center, rgba(0, 0, 0, .1) 0%, rgba(255, 0, 0, 0.2) 25%, rgba(0, 0, 0, 0) 50%, rgba(0, 0, 0, 0) 100%)',
  height: '70vh',
  width: '70vh',
  borderRadius: '50%',
  overflow: 'hidden',
  transition: '.3s ease-out all',
  transform: `rotateX(${parallax.roll * 20}deg) rotateY(${
    parallax.tilt * 20
  }deg)`,
}))
</script>

<template>
  <div ref="container" class="itemWrapper">   
    <div ref="target" :style="targetStyle">
      <div :style="containerStyle">
        <div :style="cardStyle">
          <div :style="cardWindowStyle">

            <v-icon v-if="blinkCounter" size="x-large" icon="mdi-eye-closed" class="eyeClosed"/>
            <v-icon v-else size="x-large" icon="mdi-eye" class="eyeOpen"/>

            <!-- <img
              :style="layer0"
              src="https://jaromvogel.com/images/design/jumping_rabbit/page2layer0.png"
              alt=""
            >
            <img
              :style="layer1"
              src="https://jaromvogel.com/images/design/jumping_rabbit/page2layer1.png"
              alt=""
            >
            <img
              :style="layer2"
              src="https://jaromvogel.com/images/design/jumping_rabbit/page2layer2.png"
              alt=""
            >
            <img
              :style="layer3"
              src="https://jaromvogel.com/images/design/jumping_rabbit/page2layer3.png"
              alt=""
            >
            <img
              :style="layer4"
              src="https://jaromvogel.com/images/design/jumping_rabbit/page2layer4.png"
              alt=""
            > -->
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>

.eyeOpen {
  background: radial-gradient(circle at center, 
    rgba(255, 99, 33, .99) 15%,
    rgba(3, 66, 99, 0.66) 35%,
    rgba(0, 0, 0, 0) 40%,
    rgba(0, 0, 0, 0) 100%);
  border-radius: 50%;
}

.eyeClosed {
  background: radial-gradient(circle at bottom center, 
    rgba(0, 0, 0, 0) 0%,
    rgba(0, 0, 0, 0) 62%,
    rgba(66, 33, 1, 0.1) 67%,
    rgba(0, 0, 1, 0) 80%,
    rgba(0, 0, 0, 0) 100%
    );
  border-radius: 50%;
}

.itemWrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100%;
}
</style>
