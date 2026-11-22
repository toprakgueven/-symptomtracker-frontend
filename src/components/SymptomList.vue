<template>
  <h3> {{ title }} </h3>
  <div>
    <input v-model="bezeichnungField" placeholder="Bezeichnung" type="text">
    <input v-model="staerkeField" placeholder="Stärke (1-10)" type="number" @keyup.enter="save()">
    <button type="button" @click="save()">Save</button>
  </div>
  <div>
    <table>
      <thead>
      <tr>
        <th>Bezeichnung</th>
        <th>Stärke</th>
      </tr>
      </thead>
      <tbody>
      <tr v-if="items.length === 0">
        <td colspan="2">No symptoms yet</td>
      </tr>
      <tr v-for="item in items" :key="item.id">
        <td>{{item.bezeichnung}}</td>
        <td>{{item.staerke}}</td>
      </tr>
      <tr>
        <td>{{ bezeichnungField }}</td>
        <td>{{ staerkeField }}</td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import axios from 'axios'
import type { AxiosResponse } from 'axios'
import type { Ref } from 'vue'

// Definiere den Typ passend zu deinem Backend
type Symptom = {
  id?: number
  bezeichnung: string
  staerke: number
}

defineProps<{
  title: string
}>()

const items: Ref<Symptom[]> = ref([])
const bezeichnungField = ref('')
const staerkeField = ref(1)

async function loadSymptoms() {
  const baseUrl = import.meta.env.VITE_BACKEND_BASE_URL
  const endpoint = baseUrl + '/symptoms'
  const response: AxiosResponse = await axios.get(endpoint)
  const responseData: Symptom[] = response.data
  responseData.forEach((item: Symptom) => {
    items.value.push(item)
  })
}

async function save() {
  const baseUrl = import.meta.env.VITE_BACKEND_BASE_URL
  const endpoint = baseUrl + '/symptoms'
  const data: Symptom = {
    bezeichnung: bezeichnungField.value,
    staerke: Number(staerkeField.value)
  }
  const response: AxiosResponse = await axios.post(endpoint, data)
  const responseData: Symptom = response.data
  items.value.push(responseData)
  console.log('Success:', responseData)

  // Eingabefelder zurücksetzen
  bezeichnungField.value = ''
  staerkeField.value = 1
}

// Lifecycle hook
onMounted(async () => {
  await loadSymptoms()
})
</script>

<style scoped>
h3 {
  text-align: center;
}
table {
  margin-left: auto;
  margin-right: auto;
}
button {
  color: blue;
}
</style>