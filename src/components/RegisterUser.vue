<template>
  <div>
    <input v-model="userName" placeholder="Nombre de usuario" />
    <input v-model="email" placeholder="Email" type="email" />
    <input v-model="password" placeholder="Contraseña" type="password" />
    <button @click="register">Registrarse</button>
    <div v-if="error" style="color:red">{{ error }}</div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  data() {
    return { userName: '', email: '', password: '', error: '' };
  },
  methods: {
    async register() {
      try {
        const res = await axios.post('http://localhost:8080/api/users', {
          userName: this.userName,  // ← Cambio aquí: userName en vez de username
          email: this.email,
          password: this.password
        });
        this.$emit('register-success', res.data);
        this.error = '';
      } catch (err) {
        this.error = 'No se pudo registrar el usuario';
      }
    }
  }
}
</script>
