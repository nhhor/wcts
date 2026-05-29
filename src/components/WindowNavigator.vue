<script setup lang="ts">
import { VIcon, VTooltip } from "vuetify/components";

const { title, tooltip, index } = defineProps({
  index: Number,
  title: String,
  tooltip: String,
});

// if (navigator.geolocation) {
//   navigator.geolocation.getCurrentPosition(showPosition);
// } else {
//   ("Geolocation is not supported by this browser.");
// }

// function showPosition(position: GeolocationPosition) {
//   console.log(
//     "Latitude: " +
//       position.coords.latitude +
//       " Longitude: " +
//       position.coords.longitude,
//   );
// }

const userAgentData = [
  {
    name: "Cookies Enabled",
    data: navigator.cookieEnabled,
  },
  {
    name: "Language",
    data: navigator.language,
  },
  {
    name: "Online",
    data: navigator.onLine,
  },
  {
    name: "User Agent",
    data: navigator.userAgent,
  },
];
</script>

<template>
  {{ index || 0 > 0 ? `(${index})` : "" }}
  <span class="itemWrapper">
    <v-tooltip class="tooltip" interactive :text="tooltip">
      <template #activator="{ props: tooltipProps }">
        <span v-bind="tooltipProps">
          <v-icon
            color="error"
            icon="mdi-application-brackets-outline"
            size="x-large"
          />
          <span v-if="userAgentData.length > 0" class="itemTitle"
            >{{ title }} ({{ userAgentData.length }})</span
          >
          <span v-else class="itemTitle">{{ title }}</span>
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
      <v-list v-if="userAgentData.length > 0">
        <v-list-item
          v-for="(device, index) in userAgentData"
          :key="index"
          :value="index"
          class="dataSpan2"
        >
          <v-list-item-title>
            <v-icon
              size="small"
              color="info"
              icon="mdi-application-cog-outline"
            />
            [{{ index + 1 }}] {{ device.name }}:
            <span class="dataSpan">{{ device.data }}</span>
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

li {
  width: 128px;
  font-size: 0.9rem;
  color: #666;
}
</style>
