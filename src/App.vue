<script setup>
import { ref } from 'vue'

const listTugas = ref([])
const inputTugas = ref('')

const tambahTugas = () => {
  if (inputTugas.value === '') {
    return
  }
  listTugas.value.push({
    id: listTugas.value.length + 1,
    nama: inputTugas.value,
    status: false
  })
  inputTugas.value = ''
}
</script>

<template>
  <div>
    <h1>Todo List</h1>
    <input type="text" v-model="inputTugas" />
    <button @click="tambahTugas">Tambah</button>

    <ul>
      <li v-for="tugas in listTugas" :key="tugas.id">
        {{  tugas.nama }}
      </li>
    </ul>
  </div>
</template>

<style scoped></style>
