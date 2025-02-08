<template>
  <q-route-tab
    :name="index"
    to="/comm"
    :icon="IconMap[type]"
    :label="label"
    @contextmenu.prevent
    no-caps
  >
    <q-badge color="red" v-if="received">{{ received }}</q-badge>
    <q-menu context-menu transition-show="jump-up" transition-hide="jump-down">
      <q-list style="min-width: 100px">
        <q-item clickable v-close-popup @click="onClickMenuRename">
          <q-item-section avatar>
            <q-icon color="secondary" text-color="white" name="edit" />
          </q-item-section>
          <q-item-section>Rename</q-item-section>
        </q-item>
        <q-separator />
        <q-item clickable v-close-popup @click="onClickMenuDel">
          <q-item-section avatar>
            <q-icon color="negative" text-color="white" name="delete" />
          </q-item-section>
          <q-item-section>Delete</q-item-section>
        </q-item>
      </q-list>
    </q-menu>
  </q-route-tab>
</template>

<script setup lang="ts">
import { useQuasar } from 'quasar'
import { ref } from 'vue'

export interface ConnectTabProp {
  index: string
  label: string
  type: 'TCP_CLIENT' | 'TCP_SERVER' | 'SERIAL_PORT'
  received: number
}

const IconMap = {
  TCP_CLIENT: 'mdi-lan-connect',
  TCP_SERVER: 'mdi-server-network',
  SERIAL_PORT: 'mdi-serial-port',
}

const prop = defineProps<ConnectTabProp>()
const label = ref(prop.label)
const emit = defineEmits(['delete'])
const $q = useQuasar()

/**
 * Click Menu rename button
 */
function onClickMenuRename() {
  $q.dialog({
    title: ' Rename',
    message: 'Input new tab name (between 3 and 15 characters)',
    prompt: {
      model: label.value,
      isValid: (val) => val.length > 2 && val.length < 16,
      type: 'text',
    },
    cancel: true,
  }).onOk((data) => {
    label.value = data
  })
}

/**
 * Click menu delete button
 */
function onClickMenuDel() {
  $q.dialog({
    title: 'Delete tab ',
    message: 'Do you want to remove tab ' + label.value,
    cancel: {
      push: true,
    },
    ok: {
      label: 'delete',
      icon: 'delete',
      push: true,
      color: 'negative',
    },
  }).onOk(() => {
    emit('delete', prop.index)
  })
}
</script>
