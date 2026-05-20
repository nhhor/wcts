<script setup lang="ts">
import { ref } from 'vue'
import { onKeyUp } from '@vueuse/core'
import { VIcon, VTooltip } from "vuetify/components";

const {title, tooltip, index} = defineProps({
  index: Number,
  title: String,
  tooltip: String
})

const pressedKey = ref([''])

onKeyUp(true, (e) => {
    if (pressedKey.value.length > 5) {
      pressedKey.value.splice(0, 2)
      pressedKey.value.unshift('...')
    }
    pressedKey.value = [...pressedKey.value, e.key]
})

</script>

<template>
		<v-tooltip :text="tooltip" v-slot:activator="{ props }" interactive>
      {{ index || 0 > 0 ? `(${index})` : '' }}
			<span class="itemWrapper" v-bind="props">   
        <v-icon v-if="pressedKey.join('') === ''" color="success" icon="mdi-keyboard" size="x-large"/>
        <v-icon v-else color="error" icon="mdi-keyboard" size="x-large"/>
        <span class="itemTitle">{{ title }}</span>     
        <p>[{{ pressedKey.join('') }}]</p>
      </span>
    </v-tooltip> 
</template>

<style scoped>
.itemWrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
  background: rgba(255, 255, 255, 0.5);
  border-radius: 50%;
}

.itemTitle {
 text-align: center;
 font-weight: 700;
}

p {
  text-align: center;
  background: rgba(255, 255, 255, 0.5);
  border-radius: 0.25rem;
  font-size: 0.9rem;
  color: #666;
}
</style>
