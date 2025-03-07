<template>
  <div class="login-container">
    <h2>Connexion</h2>
    <form @submit.prevent="login">
      <input v-model="loginData.email" type="email" placeholder="Email" required />
      <input v-model="loginData.password" type="password" placeholder="Mot de passe" required />
      <button type="submit">Se connecter</button>
    </form>

    <h2>Inscription</h2>
    <form @submit.prevent="register">
      <input v-model="registerData.email" type="email" placeholder="Email" required />
      <input v-model="registerData.password" type="password" placeholder="Mot de passe" required />
      <button type="submit">S'inscrire</button>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';
import axios from 'axios';

const router = useRouter();

const loginData = ref({ email: '', password: '' });
const registerData = ref({ email: '', password: '' });

const login = async () => {
  try {
    const response = await axios.post('https://pokemon-api-seyrinian-production.up.railway.app/auth/login', loginData.value);
    localStorage.setItem('token', response.data.token);
    localStorage.setItem('userId', response.data.userId);
    router.push('/deckbuilder');
  } catch (error) {
    console.error('Erreur de connexion', error);
  }
};

const register = async () => {
  try {
    await axios.post('https://pokemon-api-seyrinian-production.up.railway.app/auth/register', registerData.value);
    loginData.value.email = registerData.value.email;
    loginData.value.password = '';
    router.push('/login');
  } catch (error) {
    console.error('Erreur d'inscription', error);
  }
};
</script>

<style scoped>
.login-container {
  max-width: 400px;
  margin: auto;
  padding: 20px;
  text-align: center;
}
input {
  display: block;
  width: 100%;
  margin: 10px 0;
  padding: 8px;
}
button {
  width: 100%;
  padding: 10px;
  background-color: #007bff;
  color: white;
  border: none;
  cursor: pointer;
}
button:hover {
  background-color: #0056b3;
}
</style>
