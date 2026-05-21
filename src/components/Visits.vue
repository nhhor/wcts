<script setup lang="ts">
import { computed } from "vue";
import { useStorage } from "@vueuse/core";
import { VIcon, VTooltip } from "vuetify/components";

const { title, tooltip, index } = defineProps({
  index: Number,
  title: String,
  tooltip: String,
});

const visitData = useStorage<{ visitTimes: string[] }>("visitData", {
  visitTimes: [],
});

const visitTimes = computed<string[]>({
  get: () => visitData.value.visitTimes,
  set: (value: string[]) => {
    visitData.value.visitTimes = value;
  },
});

const bumpVisitOnMount = () => {
  visitTimes.value.push(new Date().toISOString());
};
</script>

<template>
  {{ bumpVisitOnMount() }}
  {{ index || 0 > 0 ? `(${index})` : "" }}
  <span class="itemWrapper">
    <v-tooltip class="tooltip" interactive :text="tooltip">
      <template #activator="{ props: tooltipProps }">
        <span v-bind="tooltipProps">
          <v-icon
            v-if="visitTimes.length > 1"
            icon="mdi-counter"
            size="x-large"
            color="error"
          />
          <v-icon v-else icon="mdi-counter" size="x-large" color="success" />
          <span class="itemTitle">{{ title }} ({{ visitTimes.length }})</span>
        </span>
      </template>
    </v-tooltip>
    <v-menu>
      <template #activator="{ props: menuProps }">
        <v-icon
          color="info"
          icon="mdi-information-outline"
          size="small"
          v-bind="menuProps"
        />
      </template>
      <v-list>
        <template
          v-for="(visit, index) in visitTimes"
          :key="index"
          :value="index"
        >
          <v-list-item v-if="index < 4 || index >= visitTimes.length - 5">
            ({{ index + 1 }})
            {{ new Date(visit).toLocaleString([], { timeZoneName: "short" }) }}
          </v-list-item>
          <v-list-item v-else-if="index === 5">
            <pre>   ...</pre>
          </v-list-item>
        </template>
      </v-list>
    </v-menu>
  </span>
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
  span {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }
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
