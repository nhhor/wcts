<script setup lang="ts">
import { computed } from 'vue'
import { usePermission } from '@vueuse/core'
import { VIcon, VTooltip } from "vuetify/components";

const {title, tooltip, index} = defineProps({
  index: Number,
  title: String,
  tooltip: String
})

const permissions = 
// computed(() => 
[
  { key: "accelerometer", obj: usePermission('accelerometer') },
  { key: "accessibilityEvents", obj: usePermission('accessibility-events') },
  { key: "ambientLightSensor", obj: usePermission('ambient-light-sensor') },
  { key: "backgroundSync", obj: usePermission('background-sync') },
  { key: "camera", obj: usePermission('camera') },
  { key: "clipboardRead", obj: usePermission('clipboard-read') },
  { key: "clipboardWrite", obj: usePermission('clipboard-write') },
  { key: "geolocation", obj: usePermission('geolocation') },
  { key: "gyroscope", obj: usePermission('gyroscope') },
  { key: "magnetometer", obj: usePermission('magnetometer') },
  { key: "microphone", obj: usePermission('microphone') },
  { key: "notifications", obj: usePermission('notifications') },
  { key: "paymentHandler", obj: usePermission('payment-handler') },
  { key: "persistentStorage", obj: usePermission('persistent-storage') },
  { key: "push", obj: usePermission('push') },
  { key: "speaker", obj: usePermission('speaker') },
  { key: "localFonts", obj: usePermission('local-fonts') },
]
// );

</script>

<template>
    <v-tooltip :text="tooltip" v-slot:activator="{ props: tooltip }" interactive>
      {{ index || 0 > 0 ? `(${index})` : '' }}
      <span class="itemWrapper" v-bind="tooltip">
        <v-menu>
          <template v-slot:activator="{ props: menu }">
            <v-icon v-bind="menu" color="info" icon="mdi-menu" size="x-large"/>
            <span class="itemTitle">{{ title }}</span>     
          </template>
          <v-list>
            <v-list-item v-for="(permission, index) in permissions" :key="index" :value="index">
              <v-list-item-title v-if="permission.obj.value === 'prompt'">
                ({{ index + 1 }}) {{permission.key}}: <span class="prompt">{{ permission.obj.value }}</span>
              </v-list-item-title>
              <v-list-item-title  v-else>
                ({{ index + 1 }}) {{permission.key}}: <span class="denied">{{ permission.obj.value }}</span>
              </v-list-item-title >
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
 font-weight: 700;
}

li {
  background: rgba(255, 255, 255, 0.5);
  border-radius: 0.25rem;
  font-size: 0.9rem;
  color: #666;
}

.prompt {
  color: green;
}
.denied {
  color: red;
}
</style>
