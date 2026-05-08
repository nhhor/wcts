<script setup lang="ts">
// import { useMouse } from '@vueuse/core'
import { useDevicesList } from '@vueuse/core'
import { VIcon, VTooltip } from "vuetify/components";

const {title, tooltip, index} = defineProps({
  index: Number,
  title: String,
  tooltip: String
})

const {
  devices,
  videoInputs: cameras,
  audioInputs: microphones,
  audioOutputs: speakers,
} = useDevicesList({ requestPermissions: true })

</script>

<template>
		<v-tooltip :text="tooltip" v-slot:activator="{ props }" interactive>
      {{ index || 0 > 0 ? `(${index})` : '' }}
			<span class="itemWrapper" v-bind="props">
        <v-menu>
          <template v-slot:activator="{ props: menu }">
            <span class="itemWrapper" v-bind="menu">
              <v-icon color="error" icon="mdi-devices" size="x-large"/>
              <span v-if="devices.length > 0" class="itemTitle">{{ title }} ({{ devices.length }})</span>
              <span v-else class="itemTitle">{{ title }}</span>
              <p v-if="devices.length == 0">No devices found.</p>
            </span>
          </template>
          <v-list v-if="devices.length > 0">
            <v-list-item v-for="(device, index) in devices" :key="index" :value="index">
              <v-list-item-title>
                <v-icon size="small" color="info" icon="mdi-camera" v-if="device.kind === 'videoinput'"/>
                <v-icon size="small" color="info" icon="mdi-microphone" v-else-if="device.kind === 'audioinput'"/>
                <v-icon size="small" color="info" icon="mdi-speaker" v-else-if="device.kind === 'audiooutput'"/>
                [{{ index + 1 }}] {{ device.kind }}
              </v-list-item-title>
              <ul v-if="device.deviceId || device.label || device.groupId">
                <li>
                  <span v-if="device.deviceId">{...{{ device.deviceId.substring(device.deviceId.length - 5) }}}: </span>
                  <span v-if="device.label">{{ device.label }}y </span>
                  <span v-if="device.groupId">{{ device.groupId }}z </span>
                </li>
              </ul>
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
