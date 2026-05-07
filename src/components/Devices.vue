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
        <v-icon color="error" icon="mdi-devices" />
        <span v-if="devices.length > 0" class="itemTitle">{{ title }} ({{ devices.length }})</span>
        <span v-else class="itemTitle">{{ title }}</span>
        <p v-if="devices.length == 0">No devices found.</p>
        <ul v-if="devices.length > 0">
          <li v-for="(device, index) in devices" :key="index">
            <v-icon size="small" color="info" icon="mdi-camera" v-if="device.kind === 'videoinput'"/>
            <v-icon size="small" color="info" icon="mdi-microphone" v-else-if="device.kind === 'audioinput'"/>
            <v-icon size="small" color="info" icon="mdi-speaker" v-else-if="device.kind === 'audiooutput'"/>
            [{{ device.deviceId || index+1 }}] {{ device.kind }}</li>
        </ul>
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

li {
  width: 128px;
  font-size: 0.9rem;
  color: #666;
}
</style>
