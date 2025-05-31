<template>
  <div>
    <input v-model="name" placeholder="Nombre del pedal" />
    <select v-model="type">
      <option>DYNAMICS</option>
      <option>MODULATION</option>
      <option>ECO</option>
    </select>
    <input v-model="parameters" placeholder='Parámetros JSON (ej: {"mix":80})' />
    <button @click="addPedal">Agregar Pedal</button>
    <div v-if="error" style="color:red">{{ error }}</div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  props: ['pedalboardId'],
  data() {
    return { name: '', type: 'DYNAMICS', parameters: '{}', error: '' };
  },
  methods: {
    async addPedal() {
      try {
        await axios.post('http://localhost:8080/api/pedals', {
          name: this.name,
          type: this.type,
          parameters: JSON.parse(this.parameters),
          pedalBoardId: this.pedalboardId
        });
        this.name = '';
        this.parameters = '{}';
        this.$emit('added');
        this.error = '';
      } catch (err) {
        this.error = 'Error al agregar el pedal';
      }
    }
  }
}
</script>
