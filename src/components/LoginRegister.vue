<template>
  <div>
    <div>
      <button @click="mode = 'login'">Iniciar sesión</button>
      <button @click="mode = 'register'">Registrarse</button>
    </div>
    <UserLogin v-if="mode === 'login'" @login-success="$emit('login-success', $event)" />
    <RegisterUser v-if="mode === 'register'" @register-success="onRegisterSuccess" />
  </div>
</template>

<script>
import UserLogin from './UserLogin.vue';
import RegisterUser from './RegisterUser.vue';

export default {
  components: { UserLogin, RegisterUser },
  data() {
    return { mode: 'login' };
  },
  methods: {
    onRegisterSuccess(user) {
      // Puedes iniciar sesión automáticamente tras registrar, o cambiar a login
      this.mode = 'login';
      alert('Usuario registrado correctamente. Ahora puedes iniciar sesión.');
    }
  }
}
</script>
