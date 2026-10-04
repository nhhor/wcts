<script setup lang="ts">
import { onBeforeMount, ref } from "vue";
import { VIcon, VTooltip } from "vuetify/components";

const { title, tooltip, index } = defineProps({
  index: Number,
  title: String,
  tooltip: String,
});

function titleize(s: string): string {
  if (s.length < 4) {
    return s.toUpperCase();
  }
  return s
    .toLowerCase()
    .split(" ")
    .map((word: string) => word.charAt(0).toUpperCase() + word.slice(1))
    .join(" ");
}

const ipData = ref<{
  readme?: string;
  status?: string;
  ip?: string;
  postal?: string;
  city?: string;
}>({ status: "Loading..." });

onBeforeMount(() => {
  fetch("https://ipinfo.io/json")
    .then((response) => response.json())
    .then((data) => {
      ipData.value = { ...data };
      delete ipData.value.readme;
    })
    .catch((error) => {
      console.error("Error fetching IP address:", error);
    });
});
</script>

<template>
  {{ index || 0 > 0 ? `(${index})` : "" }}

  <span class="itemWrapper">
    <v-tooltip class="tooltip" interactive :text="tooltip">
      <template #activator="{ props: tooltipProps }">
        <span v-bind="tooltipProps">
          <v-icon color="error" icon="mdi-network-pos" size="x-large" />
          <span class="itemTitle">{{ title }}</span>

          alert-octagon-outline

          {{ ipData?.ip || ipData?.status }}
          <span class="itemFooter">{{
            `${ipData?.postal ? ipData?.city + ", " + ipData?.postal : ""}`
          }}</span>
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
        <v-list-item v-for="(data, key) in ipData" :key="key" class="dataSpan2">
          <v-list-item-title>
            <v-icon
              v-if="key === 'ip'"
              size="small"
              color="info"
              icon="mdi-ip-network-outline"
            />
            <v-icon
              v-else
              size="small"
              color="white"
              icon="mdi-ip-network-outline"
            />
            <span class="dataSpan">{{ " " }}{{ titleize(key) }}</span
            ><span>{{ ": " }}{{ data }}</span>
          </v-list-item-title>
        </v-list-item>
      </v-list>
    </v-menu>
  </span>
</template>

<style scoped>
.dataSpan {
  font-weight: 600;
  /* max-width: 500px; */
  /* display: block; */
  overflow: hidden;
  text-overflow: ellipsis;

  /* display: flex; */
  /* flex-direction: row; */
  /* align-items: center; */
  /* justify-content: center; */
  /* text-align: center; */
}

.itemWrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
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
  font-weight: 700;
}

.itemFooter {
  font-weight: 100;
}

p {
  text-align: center;
  background: rgba(255, 255, 255, 0.5);
  border-radius: 0.25rem;
  font-size: 0.9rem;
  color: #666;
}

li {
  width: 128px;
  font-size: 0.9rem;
  color: #666;
}
</style>
