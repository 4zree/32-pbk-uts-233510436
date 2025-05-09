<script setup>
import { ref, computed } from 'vue'

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

const countActive = computed(() => {
  return listTugas.value.filter(tugas => !tugas.status).length
})

const hapusTugas = (id) => {
  listTugas.value = listTugas.value.filter(tugas => tugas.id !== id)
}

const filter = ref('all')
const listTugasFilter = computed(() => {
  if (filter.value === 'all') {
    return listTugas.value
  } else if (filter.value === 'active') {
    return listTugas.value.filter(tugas => !tugas.status)
  } else if (filter.value === 'completed') {
    return listTugas.value.filter(tugas => tugas.status)
  }
})

</script>

<template>
  <div>
    <h1>Todo List</h1>
    <input type="text" v-model="inputTugas" />
    <button @click="tambahTugas">Tambah</button>
    <div>
      <button @click="filter = 'all'">Semua</button>
      <button @click="filter = 'active'">Aktif</button>
      <button @click="filter = 'completed'">Selesai</button>
    </div>

    <ul>
      <li v-for="tugas in listTugasFilter" :key="tugas.id">
        <input type="checkbox" v-model="tugas.status" />
        {{ tugas.nama }}
        <button @click="hapusTugas(tugas.id)">Hapus</button>
      </li>
    </ul>
    <p>Jumlah tugas aktif: {{ countActive }}</p>
  </div>
</template>

<style scoped></style>
