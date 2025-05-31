<template>
  <div>
    <h2>Mis Pedaleras</h2>
    <ul>
      <li v-for="pb in pedalboards" :key="pb.pedalBoardId" @click="$emit('select', pb)">
        {{ pb.pedalBoardName }}
      </li>
    </ul>
    <input v-model="newName" placeholder="Nueva pedalera" />
    <button @click="addPedalboard">Agregar</button>
    <div v-if="error" style="color:red;">{{ error }}</div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  props: ['userId'],
  data() {
    return { pedalboards: [], newName: '', error: '' };
  },
  mounted() {
    this.loadPedalboards();
  },
  watch: {
    userId() {
      this.loadPedalboards();
    }
  },
  methods: {
    async loadPedalboards() {
      if (!this.userId) {
        this.pedalboards = [];
        return;
      }
      try {
        const res = await axios.get(`http://localhost:8080/api/users/${this.userId}/pedalboards`);
        this.pedalboards = res.data;
        this.error = '';
      } catch (err) {
        this.error = 'No se pudieron cargar las pedaleras';
      }
    },
    async addPedalboard() {
      if (!this.newName) {
        this.error = 'El nombre no puede estar vacío';
        return;
      }
      try {
        await axios.post(`http://localhost:8080/api/pedalboards`, {
          pedalBoardName: this.newName,  // ← Cambiado de 'name' a 'pedalBoardName'
          userId: this.userId
        });
        this.newName = '';
        this.loadPedalboards();
        this.error = '';
      } catch (err) {
        this.error = 'Error al agregar la pedalera';
      }
    }
  }
}
</script>
