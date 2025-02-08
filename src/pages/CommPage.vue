<template>
  <q-page class="row">
    <q-form class="col-3 q-gutter-sm connect-set column" @submit="onConnect" @reset="onReset">
      <q-input
        class="col-1"
        v-model="ipMod"
        clearable
        type="text"
        :shadow-text="ipInputShadow"
        lazy-rules
        :rules="ipRuler"
        label="IP"
      />
      <q-input
        class="col-1"
        v-model="portMod"
        type="number"
        lazy-rules
        :rules="portRuler"
        label="Port"
      />
      <div class="col-1 row justify-between">
        <q-btn class="col-4" label="Connect" type="submit" color="primary" />
        <q-btn class="col-4" label="Reset" type="reset" color="info" />
      </div>
    </q-form>
    <div class="col"></div>
  </q-page>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue'

const ipMod = ref('')
const ipInputShadow = computed(() => {
  const t = 'input ip address'
  const empty = typeof ipMod.value !== 'string' || ipMod.value.length === 0
  if (empty) {
    return t
  }
  return ''
})
const ipReg = /^((2(5[0-5]|[0-4]\d))|[0-1]?\d{1,2})(\.((2(5[0-5]|[0-4]\d))|[0-1]?\d{1,2})){3}$/

const ipRuler = [
  (val: string) => (val && val.length > 0) || 'please input ip address',
  (val: string) => ipReg.test(val) || 'please input valid ip address',
]

const portMod = ref(0)
const portRuler = [(val: number) => (val >= 0 && val <= 65535) || 'please input valid port']

function onReset() {}

function onConnect() {}
</script>

<style scoped>
.connect-set {
  background-color: slategray;
}
</style>
