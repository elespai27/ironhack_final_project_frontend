<template>
  <div v-if="pedalboard">
    <h3>{{ pedalboard.pedalBoardName }}</h3>
    <ul>
      <li v-for="pedal in pedals" :key="pedal.pedalId">
        {{ pedal.pedalName }} ({{ pedal.pedalType }}) 
        <span v-if="pedal.pedalBypass">[Bypass]</span>
        <span v-else>[Activo]</span>
        <br>
        <small>Parámetros: {{ pedal.pedalParameters }}</small>
      </li>
    </ul>
    <PedalForm :pedalboardId="pedalboard.pedalBoardId" @added="loadPedalboard" />
    <div v-if="error" style="color:red;">{{ error }}</div>
  </div>
</template>

<script>
import axios from 'axios';
import PedalForm from './PedalForm.vue';

export default {
  props: ['pedalboard'],
  components: { PedalForm },
  data() {
    return { pedals: [], error: '' };
  },
  watch: {
    pedalboard: 'loadPedalboard'
  },
  methods: {
    async loadPedalboard() {
      if (!this.pedalboard) return;
      try {
        const res = await axios.get(`http://localhost:8080/api/pedalboards/${this.pedalboard.pedalBoardId}`);
        this.pedals = res.data.pedals || [];
        this.error = '';
      } catch (err) {
        this.error = 'No se pudieron cargar los pedales';
      }
    }
  },
  mounted() {
    this.loadPedalboard();
  }
}
</script>
