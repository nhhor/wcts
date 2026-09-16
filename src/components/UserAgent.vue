<script setup lang="ts">
import { VIcon, VTooltip } from "vuetify/components";

const { title, tooltip, index } = defineProps({
  index: Number,
  title: String,
  tooltip: String,
});

const userAgentData = navigator.userAgent.split(/[\(\)]/);
</script>

<template>
  {{ index || 0 > 0 ? `(${index})` : "" }}
  <span class="itemWrapper">
    <v-tooltip class="tooltip" interactive :text="tooltip">
      <template #activator="{ props: tooltipProps }">
        <span v-bind="tooltipProps">
          <v-icon
            color="error"
            icon="mdi-badge-account-outline"
            size="x-large"
          />
          <span v-if="userAgentData.length > 0" class="itemTitle">{{
            title
          }}</span>
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
      <v-list>
        <v-list-item
          v-for="(data, index) in userAgentData"
          :key="index"
          class="dataSpan2"
        >
          <v-list-item-title>
            <v-icon
              v-if="index == 1"
              size="small"
              color="info"
              icon="mdi-card-account-mail-outline"
            />
            <v-icon
              v-else
              size="small"
              color="white"
              icon="mdi-card-account-mail-outline"
            />
            <span class="dataSpan">{{ " " }}{{ data }}</span>
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
