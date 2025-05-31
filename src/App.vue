<template>
  <div>
    <div v-if="!user">
      <div style="margin-bottom: 1em;">
        <button @click="mode = 'login'" :disabled="mode === 'login'">Iniciar sesión</button>
        <button @click="mode = 'register'" :disabled="mode === 'register'">Registrarse</button>
      </div>
      <UserLogin v-if="mode === 'login'" @login-success="onLoginSuccess" />
      <RegisterUser v-if="mode === 'register'" @register-success="onRegisterSuccess" />
    </div>
    <div v-else>
      <PedalboardList :userId="user.userId" @select="selected = $event" />
      <PedalboardDetail v-if="selected" :pedalboard="selected" />
      <button @click="logout" style="margin-top: 1em;">Cerrar sesión</button>
    </div>
  </div>
</template>

<script>
import UserLogin from './components/UserLogin.vue';
import RegisterUser from './components/RegisterUser.vue';
import PedalboardList from './components/PedalboardList.vue';
import PedalboardDetail from './components/PedalboardDetail.vue';

export default {
  components: { UserLogin, RegisterUser, PedalboardList, PedalboardDetail },
  data() {
    return {
      user: null,
      selected: null,
      mode: 'login' // 'login' o 'register'
    };
  },
  methods: {
    onLoginSuccess(user) {
      this.user = user;
      this.selected = null;
    },
    onRegisterSuccess(user) {
      this.user = user;
      this.selected = null;
      // Tras registrarse, puedes iniciar sesión automáticamente o cambiar a login
      this.mode = 'login';
      alert('Usuario registrado correctamente. Ahora puedes iniciar sesión.');
    },
    logout() {
      this.user = null;
      this.selected = null;
      this.mode = 'login';
    }
  }
};
</script>
