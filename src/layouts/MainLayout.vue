<template>
  <q-layout view="hHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn flat dense round icon="menu" aria-label="Menu" @click="toggleLeftDrawer" />
        <q-toolbar-title> Socket Tool </q-toolbar-title>
        <!-- <q-s pace /> -->
        <q-select
          style="width: 20%"
          v-model="locale"
          :options="languageOption"
          :label="$t('language_select')"
          filled
          emit-value
        />
      </q-toolbar>
      <q-tabs align="left" inline-label v-model="TabSelected" dense narrow-indicator>
        <ConnectTab
          v-for="Tab in TabList"
          :key="Tab.index"
          v-bind="Tab"
          @delete="onDeleteTab"
        ></ConnectTab>
      </q-tabs>
    </q-header>

    <q-drawer v-model="leftDrawerOpen" show-if-above bordered :breakpoint="500">
      <q-list>
        <q-item-label header> Connection Type </q-item-label>
        <ConnectComponent
          v-for="conn in connectTypeList"
          :key="conn.title"
          v-bind="conn"
          @item-click="OnItemClick"
        ></ConnectComponent>
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script setup lang="ts">
import type { Ref } from 'vue'
import { ref } from 'vue'
import ConnectTab, { type ConnectTabProp } from 'src/components/ConnectTab.vue'
import ConnectComponent, { type ConnectCompProp } from 'src/components/ConnectComponent.vue'
import { useQuasar } from 'quasar'
import type { ConnectType } from 'src/common/public'
import { useI18n } from 'vue-i18n'

const $q = useQuasar()
const { locale } = useI18n({ useScope: 'global' })

const connectTypeList: ConnectCompProp[] = [
  {
    title: 'Client',
    caption: 'Create socket client',
    type: 'TCP_CLIENT',
    icon: 'mdi-lan-connect',
  },
  {
    title: 'Server',
    caption: 'Create socket server',
    type: 'TCP_SERVER',
    icon: 'mdi-server-network',
  },
  {
    title: 'Serial Port',
    caption: 'Create serial port connect',
    type: 'SERIAL_PORT',
    icon: 'mdi-serial-port',
  },
]

const languageOption = [
  { value: 'en-US', label: 'English' },
  { value: 'zh-CN', label: '中文' },
]

// const languageOption = ['zh-CN', 'en-US']

const leftDrawerOpen = ref(false)
let TabIndex: number = 0
const TabSelected = ref('')
const TabList: Ref<ConnectTabProp[]> = ref([])
function toggleLeftDrawer() {
  leftDrawerOpen.value = !leftDrawerOpen.value
}

function OnItemClick(type: ConnectType) {
  TabList.value.push({
    label: 'Test',
    index: '' + TabIndex++,
    type: type,
    received: 0,
  })
  TabSelected.value = '' + TabIndex
  $q.notify(type + ' Connect tab add')
}

function onDeleteTab(index: string) {
  TabList.value = TabList.value.filter((val) => val.index !== index)
  TabSelected.value = TabList.value[0]?.index ?? ''
}
</script>
