<script setup lang="ts">
import { computed } from 'vue'
import { useStorage } from '@vueuse/core'
import { VIcon, VTooltip } from "vuetify/components";

const {title, tooltip, index} = defineProps({
  index: Number,
  title: String,
  tooltip: String
})

const visitData = useStorage<{ visitCount: number; visitTimes: string[] }>('visitData', { visitCount: 0, visitTimes: [] })
const visitCount = computed({
  get: () => visitData.value.visitCount,
  set: (value: number) => {
    visitData.value.visitCount = value
  }
})
const visitTimes = computed<string[]>({
  get: () => visitData.value.visitTimes,
  set: (value: string[]) => {
    visitData.value.visitTimes = value
  }
})

const addVisitTime = () => {
  visitTimes.value.push(new Date().toISOString())
}

// Increment visit count and add current visit time:
visitCount.value += 1
addVisitTime()
//

</script>

<!-- <template>
  {{ index || 0 > 0 ? `(${index})` : '' }}
  <v-tooltip :text="tooltip" v-slot:activator="{ props }" interactive>

    <span class="itemWrapper" v-bind="props">   
      <v-icon v-if="visitCount > 1" color="error" icon="mdi-counter" />
      <v-icon v-else color="success" icon="mdi-counter" />
      <span class="itemTitle">{{ title }} ({{ visitCount }})</span>     
      <v-menu v-if="visitTimes.length > 1">
        <template v-slot:activator="{ props: menu }">
          <span v-bind="menu">
            <v-icon v-bind="menu" color="info" icon="mdi-information-outline" size="x-small"/>
          </span>
        </template>
        <v-list>
          <v-list-item v-for="(visit, index) in visitTimes" :key="index" :value="index">
              ({{ index + 1 }}) {{new Date(visit).toLocaleString([], { timeZoneName: "short" })}}
          </v-list-item>
        </v-list>
      </v-menu>
    </span>
    
  </v-tooltip> 
</template> -->


<template>
    {{ index || 0 > 0 ? `(${index})` : '' }}
    <v-tooltip :text="tooltip" v-slot:activator="{ props: tooltip }" interactive>
      <span class="itemWrapper" v-bind="tooltip">
        <v-menu>
          <template v-slot:activator="{ props: menu }">
            <span class="itemWrapper" v-bind="menu">
              <v-icon v-if="visitCount > 1" v-bind="menu" icon="mdi-counter" size="x-large" color="error" />
              <v-icon v-else v-bind="menu" icon="mdi-counter" size="x-large" color="success" />
              <span class="itemTitle">{{ title }} ({{ visitCount }})</span>
              <v-icon color="info" icon="mdi-information-outline" size="x-small"/>

            </span>
          </template>
          <v-list>
            <v-list-item v-for="(visit, index) in visitTimes" :key="index" :value="index">
                ({{ index + 1 }}) {{new Date(visit).toLocaleString([], { timeZoneName: "short" })}}
            </v-list-item>
          </v-list>
        </v-menu>
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
