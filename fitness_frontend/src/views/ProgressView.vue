<script setup lang="ts">
import { ref } from 'vue';

const stats = ref({
  workoutsThisWeek: 4,
  totalHours: 10.5,
  goalCompletion: 65,
  topExercise: 'Cardio',
  streak: 7,
});

// Simulate graphs and stats loading
const chartLoading = ref(false);

function reloadStats() {
  chartLoading.value = true;
  setTimeout(() => {
    chartLoading.value = false;
  }, 400);
}
</script>

<template>
  <div class="progress-view">
    <header>
      <h2>Progress Dashboard</h2>
      <button @click="reloadStats" class="secondary">Refresh</button>
    </header>
    <div class="summary-cards">
      <div class="progress-card">
        <div class="progress-label">Workouts This Week</div>
        <div class="progress-val">{{ stats.workoutsThisWeek }}</div>
      </div>
      <div class="progress-card">
        <div class="progress-label">Total Hours</div>
        <div class="progress-val">{{ stats.totalHours }}</div>
      </div>
      <div class="progress-card">
        <div class="progress-label">Goal Completion</div>
        <div class="progress-val">
          <span class="goal-progress">{{ stats.goalCompletion }}%</span>
        </div>
      </div>
      <div class="progress-card">
        <div class="progress-label">Top Exercise</div>
        <div class="progress-val">{{ stats.topExercise }}</div>
      </div>
      <div class="progress-card">
        <div class="progress-label">Streak</div>
        <div class="progress-val streak">{{ stats.streak }} days</div>
      </div>
    </div>
    <div class="charts" v-if="!chartLoading">
      <div class="placeholder-chart">
        <div>Weekly Activity Graph Placeholder</div>
        <div class="fakechart"></div>
      </div>
    </div>
    <div v-else class="loading-indicator">Refreshing...</div>
  </div>
</template>

<style scoped>
.progress-view {
  max-width: 950px;
  margin: 0 auto;
}
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
button.secondary {
  background: var(--secondary, #2E7D32);
  color: #fff;
  border: none;
  padding: 0.5em 1em;
  border-radius: 6px;
  cursor: pointer;
  font-weight: 500;
}
.summary-cards {
  display: flex;
  flex-wrap: wrap;
  gap: 1.3em 2em;
  margin: 2em 0 1.5em 0;
}
.progress-card {
  flex: 1 1 170px;
  min-width: 140px;
  background: #f5fbe7;
  border-radius: 10px;
  box-shadow: 0 0 6px 0 rgba(44,62,80,0.05);
  padding: 1.5em 1em 1.3em 1em;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.progress-label {
  font-size: 0.98em;
  color: #495f48;
  margin-bottom: 0.4em;
}
.progress-val {
  font-size: 1.42em;
  font-weight: 700;
  color: var(--primary, #4CAF50);
}
.progress-val.streak {
  color: var(--accent, #FFC107);
}
.goal-progress {
  color: var(--secondary, #2E7D32);
}
.charts {
  margin-top: 2em;
}
.placeholder-chart {
  width: 100%;
  max-width: 610px;
  margin: 0 auto;
  padding: 1.8em;
  background: #f4f4fc;
  border-radius: 12px;
  box-shadow: 0 1px 10px 0 rgba(44,62,80,0.09);
  text-align: center;
  margin-bottom: 2em;
}
.fakechart {
  background: linear-gradient(90deg, #4CAF50 52%, #FFC107 100%);
  width: 80%;
  height: 24px;
  border-radius: 16px;
  margin: 1.2em auto 0 auto;
  opacity: 0.28;
}
.loading-indicator {
  padding: 1.4em;
  text-align: center;
}
</style>
