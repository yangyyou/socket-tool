<template>
  <q-item clickable v-ripple @click="OnClick">
    <q-item-section left avatar>
      <q-avatar color="primary" text-color="white" :icon="icon" />
    </q-item-section>
    <q-item-section>
      <q-item-label>{{ title }}</q-item-label>
      <q-item-label caption lines="2">{{ caption }}</q-item-label>
    </q-item-section>
  </q-item>
</template>

<script setup lang="ts">
import type { ConnectType } from 'src/common/public'

export interface ConnectCompProp {
  title: string
  caption: string
  icon: string
  type: ConnectType
}
const props = defineProps<ConnectCompProp>()

const emit = defineEmits<{
  ItemClick: [type: ConnectType]
}>()

function OnClick() {
  emit('ItemClick', props.type)
}
</script>

<style scoped></style>
