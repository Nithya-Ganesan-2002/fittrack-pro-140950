<script setup lang="ts">
import { defineProps, defineEmits } from 'vue';
import { RouterLink, useRoute } from 'vue-router';

// For multi-word name (Vue style-guide)
defineOptions({ name: 'SidebarNav' });

const props = defineProps<{ isOpen: boolean }>();
const emit = defineEmits(['toggle']);
const route = useRoute();

const links = [
  { name: 'Dashboard', path: '/', icon: '🏠' },
  { name: 'Workouts', path: '/workouts', icon: '💪' },
  { name: 'Goals', path: '/goals', icon: '🎯' },
  { name: 'Progress', path: '/progress', icon: '📈' },
  { name: 'Routines', path: '/routines', icon: '🗓️' },
];
</script>

<template>
  <aside :class="{ open: props.isOpen }" id="sidebar">
    <div class="sidebar-header">
      <div class="brand">
        <div class="brand-circle"></div>
        <span>FitTrack Pro</span>
      </div>
      <button class="sidebar-toggle" @click="emit('toggle')" aria-label="Toggle sidebar">
        <span v-if="props.isOpen">⬅</span>
        <span v-else>➡</span>
      </button>
    </div>
    <nav v-if="props.isOpen">
      <ul>
        <li v-for="link in links" :key="link.path">
          <RouterLink :to="link.path" :class="{active: route.path === link.path}">
            <span class="sidebar-icon">{{ link.icon }}</span> {{link.name}}
          </RouterLink>
        </li>
      </ul>
    </nav>
  </aside>
</template>

<style scoped>
#sidebar {
  width: 220px;
  background: var(--primary, #4CAF50);
  color: #fff;
  min-height: 100vh;
  transition: width 0.25s, min-width 0.25s;
  display: flex;
  flex-direction: column;
  box-shadow: 1px 0 12px 0 rgba(44,62,80,0.07);
}

#sidebar:not(.open) {
  width: 60px;
  min-width: 60px;
}
.sidebar-header {
  display: flex;
  align-items: center;
  height: 64px;
  justify-content: space-between;
  padding: 0 1rem;
  background: var(--secondary, #2E7D32);
}

.brand {
  display: flex;
  align-items: center;
}

.brand-circle {
  background: var(--accent, #FFC107);
  width: 32px;
  height: 32px;
  border-radius: 16px;
  margin-right: 8px;
  border: 2px solid #fff;
}
.brand span {
  font-size: 1.1rem;
  font-weight: bold;
  letter-spacing: 0.02em;
  color: #fff;
}

.sidebar-toggle {
  background: transparent;
  border: none;
  color: #fff;
  font-size: 1.25rem;
  cursor: pointer;
}

nav ul {
  list-style: none;
  margin: 0;
  padding: 1.5em 0 0 0;
}
nav li {
  margin: 0;
}

nav a {
  display: flex;
  align-items: center;
  padding: 0.75em 2em 0.75em 1.5em;
  color: #fff;
  font-weight: 500;
  font-size: 1.01em;
  letter-spacing: 0.01em;
  text-decoration: none;
  transition: background 0.14s, color 0.16s;
  border-left: 4px solid transparent;
}

nav a.active, nav a:hover, nav a:focus {
  background: rgba(255,255,255,0.08);
  color: #FFC107;
  border-left: 4px solid #FFC107;
}

.sidebar-icon {
  font-size: 1.3em;
  width: 2.5em;
  display: inline-block;
}

@media (max-width: 900px) {
  #sidebar {
    width: 100vw;
    min-width: 0;
    height: 60px; 
    min-height: 60px;
    flex-direction: row;
    align-items: center;
  }
  .brand, nav {
    display: none;
  }
  #sidebar.open .brand, #sidebar.open nav {
    display: flex !important;
  }
}
</style>
