<template>
  <div>
    <input v-model="userId" placeholder="ID de usuario" />
    <button @click="login">Iniciar sesión</button>
    <div v-if="error" style="color:red">{{ error }}</div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  data() {
    return { userId: '', error: '' };
  },
  methods: {
    async login() {
      try {
        const res = await axios.get(`http://localhost:8080/api/user/${this.userId}`);
        this.$emit('login-success', res.data);
        this.error = '';
      } catch (err) {
        this.error = 'Usuario no encontrado';
      }
    }
  }
}
</script>
