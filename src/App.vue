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

const baseBtnClass = 'px-4 py-2 border border-gray-300 rounded-lg text-gray-700 hover:bg-gray-200 transition'
const activeBtnClass = 'px-4 py-2 bg-gray-700 text-white rounded-lg'

</script>

<template>
  <div class="min-h-screen bg-gray-100 flex items-center justify-center py-10 px-4">
    <div class="bg-white rounded-xl shadow-lg p-6 w-full max-w-2xl space-y-6">
      <h1 class="text-2xl font-bold text-gray-800 text-center">📝 Todo List</h1>

      <div class="flex gap-3">
        <input
          type="text"
          v-model="inputTugas"
          placeholder="Tambahkan tugas..."
          class="flex-1 px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-gray-500"
        />
        <button
          @click="tambahTugas"
          class="bg-gray-800 text-white px-4 py-2 rounded-lg hover:bg-gray-700 transition"
        >
          Tambah
        </button>
      </div>

      <div class="flex gap-2 justify-center">
        <button
          @click="filter = 'all'"
          :class="filter === 'all' ? activeBtnClass : baseBtnClass"
        >
          Semua
        </button>
        <button
          @click="filter = 'active'"
          :class="filter === 'active' ? activeBtnClass : baseBtnClass"
        >
          Aktif
        </button>
        <button
          @click="filter = 'completed'"
          :class="filter === 'completed' ? activeBtnClass : baseBtnClass"
        >
          Selesai
        </button>
      </div>

      <ul class="space-y-3 max-h-80 overflow-y-auto pr-2">
        <li
          v-for="tugas in listTugasFilter"
          :key="tugas.id"
          class="flex items-center justify-between bg-gray-50 border border-gray-200 rounded-lg px-4 py-2"
        >
          <div class="flex items-center gap-3">
            <input type="checkbox" v-model="tugas.status" class="accent-gray-600 w-5 h-5" />
            <span :class="tugas.status ? 'line-through text-gray-400' : 'text-gray-800'">
              {{ tugas.nama }}
            </span>
          </div>
          <button
            @click="hapusTugas(tugas.id)"
            class="text-sm text-red-500 hover:text-red-700 font-medium"
          >
            🗑️
          </button>
        </li>
      </ul>

      <p class="text-center text-gray-600 font-medium">Jumlah tugas aktif: {{ countActive }}</p>
    </div>
  </div>
</template>

<style scoped></style>
