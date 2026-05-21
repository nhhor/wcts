<script setup lang="ts">
import { usePermission } from "@vueuse/core";
import { computed } from "vue";

const { title, tooltip, index } = defineProps({
  index: Number,
  title: String,
  tooltip: String,
});

const permissions = [
  { key: "accelerometer", obj: usePermission("accelerometer") },
  { key: "accessibilityEvents", obj: usePermission("accessibility-events") },
  { key: "ambientLightSensor", obj: usePermission("ambient-light-sensor") },
  { key: "backgroundSync", obj: usePermission("background-sync") },
  { key: "camera", obj: usePermission("camera") },
  { key: "clipboardRead", obj: usePermission("clipboard-read") },
  { key: "clipboardWrite", obj: usePermission("clipboard-write") },
  { key: "geolocation", obj: usePermission("geolocation") },
  { key: "gyroscope", obj: usePermission("gyroscope") },
  { key: "magnetometer", obj: usePermission("magnetometer") },
  { key: "microphone", obj: usePermission("microphone") },
  { key: "notifications", obj: usePermission("notifications") },
  { key: "paymentHandler", obj: usePermission("payment-handler") },
  { key: "persistentStorage", obj: usePermission("persistent-storage") },
  { key: "push", obj: usePermission("push") },
  { key: "speaker", obj: usePermission("speaker") },
  { key: "localFonts", obj: usePermission("local-fonts") },
];

const grantedCount = computed(
  () =>
    (
      permissions
        .map((p) => p.obj.value)
        .join(",")
        .match(new RegExp("granted", "g")) || []
    ).length,
);
</script>

<template>
  {{ index || 0 > 0 ? `(${index})` : "" }}
  <span class="itemWrapper">
    <v-tooltip class="tooltip" interactive :text="tooltip">
      <template #activator="{ props: tooltipProps }">
        <span v-bind="tooltipProps">
          <v-icon
            v-if="grantedCount > permissions.length / 3"
            color="error"
            icon="mdi-account-outline"
            size="x-large"
          />
          <v-icon
            v-else
            color="success"
            icon="mdi-account-outline"
            size="x-large"
          />
          <span class="itemTitle"
            >{{ title }} ({{ grantedCount }}/{{ permissions.length }})
          </span>
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
          v-for="(permission, index) in permissions"
          :key="index"
          :value="index"
        >
          <v-list-item-title v-if="permission.obj.value === 'prompt'">
            ({{ index + 1 }}) {{ permission.key }}:
            <span class="prompt">{{ permission.obj.value }}</span>
          </v-list-item-title>
          <v-list-item-title v-else-if="permission.obj.value === 'denied'">
            ({{ index + 1 }}) {{ permission.key }}:
            <span class="denied">{{ permission.obj.value }}</span>
          </v-list-item-title>
          <v-list-item-title v-else>
            ({{ index + 1 }}) {{ permission.key }}:
            <span class="granted">{{ permission.obj.value }}</span>
          </v-list-item-title>
        </v-list-item>
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
  text-align: center;
  width: 100%;
  height: 100%;
  background: rgba(255, 255, 255, 0.5);
  border-radius: 50%;
}

.itemTitle {
  font-weight: 700;
}

li {
  background: rgba(255, 255, 255, 0.5);
  border-radius: 0.25rem;
  font-size: 0.9rem;
  color: #666;
}

.prompt {
  color: blue;
}

.denied {
  color: green;
}

.granted {
  color: red;
}
</style>
