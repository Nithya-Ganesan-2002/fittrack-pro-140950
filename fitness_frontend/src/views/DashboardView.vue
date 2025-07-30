<script setup lang="ts">
</script>

<template>
  <div class="dashboard-view">
    <h1>🏋️ FitTrack Pro Dashboard</h1>
    <p class="subtitle">
      Your fitness journey at a glance.
    </p>
    <div class="dashboard-cards">
      <router-link class="dashboard-card" to="/workouts">
        <div class="icon">💪</div>
        <div>Log & Review Workouts</div>
      </router-link>
      <router-link class="dashboard-card" to="/goals">
        <div class="icon">🎯</div>
        <div>Set Your Fitness Goals</div>
      </router-link>
      <router-link class="dashboard-card" to="/progress">
        <div class="icon">📈</div>
        <div>View Progress Stats</div>
      </router-link>
      <router-link class="dashboard-card" to="/routines">
        <div class="icon">🗓️</div>
        <div>Manage Routines</div>
      </router-link>
    </div>
    <div class="welcome">
      <p>
        Welcome to FitTrack Pro, your all-in-one fitness tracking and motivation tool. Use the navigation sidebar for fast access to logging workouts, reviewing your progress, setting goals, and planning weekly routines.
      </p>
    </div>
  </div>
</template>

<style scoped>
.dashboard-view {
  max-width: 1000px;
  margin: 0 auto;
  text-align: center;
  padding-top: 1em;
}
.subtitle {
  color: #7ca193;
  font-size: 1.09em;
}
.dashboard-cards {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 2em;
  margin: 2.3em 0 2em 0;
}
.dashboard-card {
  background: #f7fff6;
  border-radius: 12px;
  box-shadow: 0 1px 8px 0 rgba(44,62,80,0.05);
  width: 190px;
  height: 120px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  color: var(--primary, #4CAF50);
  font-size: 1.17em;
  font-weight: 600;
  text-decoration: none;
}
.dashboard-card .icon {
  font-size: 2.1em;
  margin-bottom: 0.5em;
}
.dashboard-card:hover, .dashboard-card:focus {
  background: #E3F9E5;
  color: var(--accent, #FFC107);
}
.welcome {
  color: #7a7a7a;
  font-size: 1.1em;
  margin-top: 2.5em;
}
</style>
