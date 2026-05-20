<script setup lang="ts">
import { usePermission } from '@vueuse/core'
import {computed} from 'vue'

const {title, tooltip, index} = defineProps({
  index: Number,
  title: String,
  tooltip: String
})

const permissions = 
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

const grantedCount = computed(() => (permissions.map(p => p.obj.value).join(',').match(new RegExp('granted', "g")) || []).length);

</script>

<template>
  {{ index || 0 > 0 ? `(${index})` : '' }}
    <v-tooltip :text="tooltip" v-slot:activator="{ props: tooltip }" interactive>
      <span class="itemWrapper" v-bind="tooltip">
        <v-menu>
          <template v-slot:activator="{ props: menu }">
            <span class="itemWrapper" v-bind="menu">
              <v-icon v-bind="menu" color="info" icon="mdi-menu" size="x-large"/>
              <span v-if="permissions.length > 0" class="itemTitle">{{ title }} ({{ grantedCount }}/{{ permissions.length }})</span>
              <span v-else class="itemTitle">{{ title }}x</span>
            </span>
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
  color: green;
}
.denied {
  color: red;
}
</style>
