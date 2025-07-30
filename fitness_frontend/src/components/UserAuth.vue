<script setup lang="ts">
import { ref, computed } from 'vue';

// Placeholder for real authentication state and API calls.
const user = ref<{email: string}|null>(null);
const loginFormOpen = ref(false);
const registerFormOpen = ref(false);

const email = ref('');
const password = ref('');
const errorMsg = ref('');
const isLoading = ref(false);

function openLogin() {
  loginFormOpen.value = true;
  registerFormOpen.value = false;
  email.value = '';
  password.value = '';
  errorMsg.value = '';
}
function openRegister() {
  loginFormOpen.value = false;
  registerFormOpen.value = true;
  email.value = '';
  password.value = '';
  errorMsg.value = '';
}

// PUBLIC_INTERFACE
function login() {
  isLoading.value = true;
  setTimeout(() => {
    if (email.value === 'test@fit.com') {
      user.value = { email: email.value };
      loginFormOpen.value = false;
      errorMsg.value = '';
    } else {
      errorMsg.value = 'Invalid credentials';
    }
    isLoading.value = false;
  }, 800);
}

// PUBLIC_INTERFACE
function register() {
  isLoading.value = true;
  setTimeout(() => {
    user.value = { email: email.value };
    registerFormOpen.value = false;
    errorMsg.value = '';
    isLoading.value = false;
  }, 800);
}

// PUBLIC_INTERFACE
function logout() {
  user.value = null;
}

const userInitials = computed(() =>
  user.value && user.value.email
    ? user.value.email.charAt(0).toUpperCase()
    : '?'
);
</script>

<template>
  <div class="user-auth">
    <template v-if="user">
      <div class="user-chip">
        <span class="user-avatar">{{ userInitials }}</span>
        <span>{{ user.email }}</span>
        <button class="auth-btn" title="Logout" @click="logout">Logout</button>
      </div>
    </template>
    <template v-else>
      <div class="auth-actions">
        <button class="auth-btn primary" @click="openLogin">Login</button>
        <button class="auth-btn accent" @click="openRegister">Sign Up</button>
      </div>
    </template>
    <div v-if="loginFormOpen" class="auth-dialog">
      <form @submit.prevent="login">
        <h3>Login</h3>
        <input v-model="email" type="email" placeholder="Email" required />
        <input v-model="password" type="password" placeholder="Password" required />
        <button :disabled="isLoading" type="submit" class="auth-btn primary">Login</button>
        <span v-if="errorMsg" class="auth-error">{{ errorMsg }}</span>
      </form>
    </div>
    <div v-if="registerFormOpen" class="auth-dialog">
      <form @submit.prevent="register">
        <h3>Register</h3>
        <input v-model="email" type="email" placeholder="Email" required />
        <input v-model="password" type="password" placeholder="Password" required />
        <button :disabled="isLoading" type="submit" class="auth-btn accent">Register</button>
        <span v-if="errorMsg" class="auth-error">{{ errorMsg }}</span>
      </form>
    </div>
  </div>
</template>

<style scoped>
.user-auth {
  display: flex;
  align-items: center;
  position: relative;
}
.user-chip {
  display: flex;
  align-items: center;
  gap: 8px;
  background: #e7f9ed;
  border-radius: 100px;
  padding: 0.25em 1em 0.25em 0.5em;
  font-size: 1em;
}
.user-avatar {
  background: var(--primary, #4CAF50);
  color: #fff;
  border-radius: 50px;
  width: 2em;
  height: 2em;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.1em;
}
.auth-actions {
  display: flex;
  gap: 0.5em;
}
.auth-btn {
  background: var(--primary, #4CAF50);
  color: #fff;
  border: none;
  padding: 0.34em 1.2em;
  border-radius: 6px;
  cursor: pointer;
  font-weight: 500;
  font-size: 1em;
  transition: background 0.15s;
}
.auth-btn.accent {
  background: var(--accent, #FFC107);
  color: #333;
}
.auth-btn:disabled {
  opacity: 0.5;
  cursor: default;
}
.auth-dialog form {
  background: #fff;
  box-shadow: 0 4px 16px 0 rgba(44,62,80,0.11);
  border-radius: 8px;
  padding: 1.5em;
  display: flex;
  flex-direction: column;
  position: absolute;
  top: 2.2em;
  right: 0;
  z-index: 1;
  gap: 0.6em;
  min-width: 210px;
}
.auth-dialog input {
  padding: 0.54em 0.8em;
  border-radius: 6px;
  border: 1.5px solid #eee;
  font-size: 1em;
}
.auth-error {
  color: #b93636;
  font-size: 0.98em;
  font-weight: 500;
}
</style>
