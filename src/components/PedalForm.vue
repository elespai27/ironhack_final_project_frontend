<template>
  <div>
    <input v-model="name" placeholder="Nombre del pedal" />
    <select v-model="type">
      <option>DYNAMICS</option>
      <option>MODULATION</option>
      <option>ECO</option>
    </select>
    <input v-model="parameters" placeholder='Parámetros JSON (ej: {"mix":80})' />
    <!-- Aquí agregas el bloque de ayuda -->
    <small style="display:block; color:#555; margin-bottom:8px;">
      Ejemplo de parámetros:<br>
      <code>
        DYNAMICS: {"mix":75, "feedback":30, "time":150}<br>
        MODULATION: {"mix":80, "rate":45}<br>
        ECO: {"mix":60, "time":300, "feedback":50}
      </code>
    </small>
    <button @click="addPedal">Agregar Pedal</button>
    <div v-if="error" style="color:red;">{{ error }}</div>
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
          pedalName: this.name,
          pedalType: this.type,
          pedalBypass: this.bypass,
          pedalParameters: JSON.stringify(this.parameters),
          pedalBoardId: this.pedalboardId
        });
        this.name = '';
        this.bypass = false;
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
