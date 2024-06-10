<template>
  <div :style="{ backgroundColor: warnaLatar }" class="container">
    <h1>Daftar Tugas</h1>
    <input v-model="tugasBaru" placeholder="Tambah tugas..." class="input-tugas"/>
    <button @click="tambahTugas" class="btn-tambah">Tambah</button>
    
    <h2>Tugas Belum Selesai</h2>
    <table v-if="tugasBelumSelesai.length > 0" class="tabel-tugas">
      <thead>
        <tr>
          <th>Tugas</th>
          <th>Aksi</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(tugas, index) in tugasBelumSelesai" :key="index">
          <td>
            <input type="checkbox" v-model="tugas.selesai" />
            <span :class="{ selesai: tugas.selesai }">{{ tugas.nama }}</span>
          </td>
          <td>
            <button @click="hapusTugas(index)" class="btn-hapus">Hapus</button>
          </td>
        </tr>
      </tbody>
    </table>
    <p v-else>Tidak ada tugas yang belum selesai</p>

    <h2>Tugas Selesai</h2>
    <table v-if="tugasSelesai.length > 0" class="tabel-tugas">
      <thead>
        <tr>
          <th>Tugas</th>
          <th>Aksi</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(tugas, index) in tugasSelesai" :key="index">
          <td>
            <input type="checkbox" v-model="tugas.selesai" />
            <span :class="{ selesai: tugas.selesai }">{{ tugas.nama }}</span>
          </td>
          <td>
            <button @click="hapusTugas(index)" class="btn-hapus">Hapus</button>
          </td>
        </tr>
      </tbody>
    </table>
    <p v-else>Tidak ada tugas yang sudah selesai</p>

    <p>{{ tugasBelumSelesai.length }} tugas belum selesai</p>
  </div>
</template>

<script>
import { ref, computed } from 'vue'
import { useCountStore } from './stores/countStore'

export default {
  setup() {
    const countStore = useCountStore()

    const tugasBaru = ref('')
    const tugas = ref([])

    const tambahTugas = () => {
      if (tugasBaru.value.trim() !== '') {
        tugas.value.push({ nama: tugasBaru.value, selesai: false })
        tugasBaru.value = ''
      }
    }

    const hapusTugas = (index) => {
      tugas.value.splice(index, 1)
    }

    const tugasBelumSelesai = computed(() => tugas.value.filter(tugas => !tugas.selesai))
    const tugasSelesai = computed(() => tugas.value.filter(tugas => tugas.selesai))

    const warnaLatar = computed(() => tugas.value.length === 0 ? 'lightcoral' : 'lightgreen')

    return {
      tugasBaru,
      tugas,
      tambahTugas,
      hapusTugas,
      tugasBelumSelesai,
      tugasSelesai,
      warnaLatar,
      countStore
    }
  }
}
</script>

<style>
.container {
  background-image: url('https://path/to/your/image.jpg'); /* Ganti dengan URL gambar yang diinginkan */
  background-size: cover;
  padding: 20px;
  border-radius: 15px;
}

.input-tugas {
  padding: 10px;
  border: 2px solid pink;
  border-radius: 5px;
  margin-right: 10px;
}

.btn-tambah {
  padding: 10px 20px;
  background-color: pink;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.btn-tambah:hover {
  background-color: darkpink;
}

.tabel-tugas {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

.tabel-tugas th, .tabel-tugas td {
  border: 1px solid pink;
  padding: 10px;
  text-align: left;
}

.tabel-tugas th {
  background-color: pink;
}

.tabel-tugas td {
  background-color: white;
}

.btn-hapus {
  padding: 5px 10px;
  background-color: lightcoral;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.btn-hapus:hover {
  background-color: darkred;
}

.selesai {
  text-decoration: line-through;
  color: gray;
}
</style>
