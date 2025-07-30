<script setup lang="ts">
import { ref } from 'vue';

interface Goal {
  id: number;
  description: string;
  target: number;
  completed: number;
  unit: string;
  period: string;
  archived?: boolean;
}

const goals = ref<Goal[]>([]);
const showForm = ref(false);
const isLoading = ref(false);

// Goal form
const form = ref<{ description: string; target: number; unit: string; period: string }>({
  description: '',
  target: 1,
  unit: 'reps',
  period: 'week',
});

// Simulate API call to get user's goals
function loadGoals() {
  isLoading.value = true;
  setTimeout(() => {
    goals.value = [
      {
        id: 1,
        description: 'Run 10 km',
        target: 10,
        completed: 5,
        unit: 'km',
        period: 'week'
      },
      {
        id: 2,
        description: 'Bench press 2x/week',
        target: 2,
        completed: 1,
        unit: 'sessions',
        period: 'week'
      }
    ];
    isLoading.value = false;
  }, 400);
}

// PUBLIC_INTERFACE
function addGoal() {
  isLoading.value = true;
  setTimeout(() => {
    goals.value.unshift({
      id: Date.now(),
      description: form.value.description,
      target: form.value.target,
      completed: 0,
      unit: form.value.unit,
      period: form.value.period,
    });
    isLoading.value = false;
    showForm.value = false;
    form.value = { description: '', target: 1, unit: 'reps', period: 'week' };
  }, 400);
}

function openForm() {
  showForm.value = true;
}

function closeForm() {
  showForm.value = false;
}

loadGoals();
</script>

<template>
  <div class="goals-view">
    <header>
      <h2>Fitness Goals</h2>
      <button @click="openForm" class="accent">Add Goal</button>
    </header>
    <div v-if="showForm" class="goal-form-dialog">
      <form @submit.prevent="addGoal">
        <h3>New Goal</h3>
        <div>
          <label>Description</label>
          <input v-model="form.description" required maxlength="100"/>
        </div>
        <div>
          <label>Target</label>
          <input v-model.number="form.target" type="number" min="1" />
        </div>
        <div>
          <label>Unit</label>
          <select v-model="form.unit">
            <option value="reps">Reps</option>
            <option value="km">Kilometers</option>
            <option value="sessions">Sessions</option>
            <option value="mins">Minutes</option>
            <option value="other">Other</option>
          </select>
        </div>
        <div>
          <label>Period</label>
          <select v-model="form.period">
            <option value="week">Week</option>
            <option value="month">Month</option>
            <option value="ever">All-Time</option>
          </select>
        </div>
        <div class="form-actions">
          <button type="submit" :disabled="isLoading" class="accent">Save</button>
          <button type="button" @click="closeForm">Cancel</button>
        </div>
      </form>
    </div>
    <div v-if="isLoading" class="loading-indicator">Loading...</div>
    <div v-else>
      <table v-if="goals.length > 0" class="goal-table">
        <thead>
          <tr>
            <th>Description</th>
            <th>Target</th>
            <th>Completed</th>
            <th>Unit</th>
            <th>Period</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="g in goals" :key="g.id">
            <td>{{ g.description }}</td>
            <td>{{ g.target }}</td>
            <td>{{ g.completed }}</td>
            <td>{{ g.unit }}</td>
            <td>{{ g.period }}</td>
          </tr>
        </tbody>
      </table>
      <div v-else class="empty-msg">No fitness goals set.</div>
    </div>
  </div>
</template>

<style scoped>
.goals-view {
  max-width: 700px;
  margin: 0 auto;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

button.accent {
  background: var(--accent, #FFC107);
  color: #222;
  border: none;
  padding: 0.5em 1em;
  border-radius: 6px;
  cursor: pointer;
  font-weight: 500;
}

.goal-form-dialog {
  background: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 18px 0 rgba(44,62,80,0.1);
  padding: 1.2em 1.5em;
  margin: 1em 0;
}

.goal-form-dialog form > div {
  margin-bottom: 0.7em;
}

.goal-form-dialog label {
  display: block;
  font-size: 1.02em;
  font-weight: 500;
  margin-bottom: 3px;
}

.goal-form-dialog input,
.goal-form-dialog select {
  width: 100%;
  padding: 0.4em;
  border-radius: 6px;
  border: 1px solid #e0e0e0;
  font-size: 1em;
}

.form-actions {
  display: flex;
  gap: 1em;
}

.goal-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 1.3em;
}

.goal-table th, .goal-table td {
  padding: 0.55em 0.6em;
  border-bottom: 1px solid #efefef;
  text-align: left;
}

.goal-table th {
  background: #f6faf6;
}
.empty-msg {
  text-align: center;
  padding: 1.2em;
}
.loading-indicator {
  padding: 1.1em;
  text-align: center;
}
</style>
