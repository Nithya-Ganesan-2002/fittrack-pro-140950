<script setup lang="ts">
import { ref } from 'vue';
import { RouterView } from 'vue-router';
import Sidebar from './components/Sidebar.vue';
import UserAuth from './components/UserAuth.vue';

const isSidebarOpen = ref(true);
</script>

<template>
  <div id="app-layout">
    <Sidebar :is-open="isSidebarOpen" @toggle="isSidebarOpen = !isSidebarOpen" />
    <main :class="{ 'sidebar-collapsed': !isSidebarOpen }">
      <div class="main-toolbar">
        <UserAuth />
      </div>
      <RouterView />
    </main>
  </div>
</template>

<style scoped>
#app-layout {
  display: flex;
  min-height: 100vh;
  background: var(--color-background, #fafafa);
}

main {
  flex: 1;
  padding: 2rem;
  background: #fff;
  transition: margin-left 0.3s;
  min-width: 0;
  box-shadow: 0 1px 8px 0 rgba(44,62,80,0.03);
}

.main-toolbar {
  width: 100%;
  display: flex;
  justify-content: flex-end;
  align-items: center;
  height: 56px;
  margin-bottom: 1.5rem;
}

@media (max-width: 900px) {
  #app-layout {
    flex-direction: column;
  }
  main {
    padding: 1rem;
  }
}
</style>
