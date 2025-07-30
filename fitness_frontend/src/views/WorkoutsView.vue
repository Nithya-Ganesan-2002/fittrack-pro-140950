<script setup lang="ts">
import { ref } from 'vue';

interface Workout {
  id: number;
  date: string;
  type: string;
  duration: number;
  notes: string;
}

const workouts = ref<Workout[]>([]);
const isLoading = ref(false);
const errorMsg = ref('');
const showForm = ref(false);

// Form inputs
const form = ref<{ date: string; type: string; duration: number; notes: string }>({
  date: '',
  type: '',
  duration: 30,
  notes: '',
});

const exerciseTypes = [
  'Strength Training', 'Cardio', 'HIIT', 'Flexibility', 'Yoga', 'Other'
];

// Placeholder: Load workouts from API
function loadWorkouts() {
  isLoading.value = true;
  // Simulated fetch
  setTimeout(() => {
    workouts.value = [
      { id: 1, date: '2024-01-14', type: 'Strength Training', duration: 45, notes: 'Push day' },
      { id: 2, date: '2024-01-15', type: 'Cardio', duration: 30, notes: 'Treadmill' },
    ];
    isLoading.value = false;
  }, 500);
}

// PUBLIC_INTERFACE
function addWorkout() {
  isLoading.value = true;
  // Placeholder for API call
  setTimeout(() => {
    workouts.value.unshift({
      id: Date.now(),
      date: form.value.date,
      type: form.value.type,
      duration: form.value.duration,
      notes: form.value.notes,
    });
    isLoading.value = false;
    showForm.value = false;
    form.value = { date: '', type: '', duration: 30, notes: '' };
  }, 400);
}

function openForm() {
  showForm.value = true;
  errorMsg.value = '';
}

function closeForm() {
  showForm.value = false;
}

loadWorkouts();
</script>

<template>
  <div class="workouts-view">
    <header>
      <h2>Workout Log</h2>
      <button @click="openForm" class="primary">Add Workout</button>
    </header>
    <div v-if="showForm" class="workout-form-dialog">
      <form @submit.prevent="addWorkout">
        <h3>New Workout</h3>
        <div>
          <label>Date</label>
          <input v-model="form.date" type="date" required />
        </div>
        <div>
          <label>Type</label>
          <select v-model="form.type" required>
            <option disabled value="">Choose...</option>
            <option v-for="t in exerciseTypes" :key="t">{{ t }}</option>
          </select>
        </div>
        <div>
          <label>Duration (min)</label>
          <input v-model.number="form.duration" type="number" min="1" step="1" required />
        </div>
        <div>
          <label>Notes</label>
          <textarea v-model="form.notes" rows="2" />
        </div>
        <div class="form-actions">
          <button type="submit" :disabled="isLoading" class="primary">Save</button>
          <button type="button" @click="closeForm">Cancel</button>
        </div>
      </form>
    </div>
    <div v-if="isLoading" class="loading-indicator">Loading...</div>
    <div v-else class="workout-list">
      <table v-if="workouts.length > 0">
        <thead>
          <tr>
            <th>Date</th>
            <th>Type</th>
            <th>Duration</th>
            <th>Notes</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="w in workouts" :key="w.id">
            <td>{{ w.date }}</td>
            <td>{{ w.type }}</td>
            <td>{{ w.duration }} min</td>
            <td>{{ w.notes }}</td>
          </tr>
        </tbody>
      </table>
      <div v-else class="empty-msg">No workouts logged yet.</div>
    </div>
  </div>
</template>

<style scoped>
.workouts-view {
  max-width: 800px;
  margin: 0 auto;
}
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
button.primary {
  background: var(--primary, #4CAF50);
  color: #fff;
  border: none;
  padding: 0.5em 1em;
  border-radius: 6px;
  cursor: pointer;
  font-weight: 500;
}
.workout-form-dialog {
  background: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 18px 0 rgba(43,59,92,0.12);
  padding: 1.2em 1.5em;
  margin: 1em 0;
}
.workout-form-dialog form > div {
  margin-bottom: 0.7em;
}
.workout-form-dialog label {
  display: block;
  font-size: 1.02em;
  font-weight: 500;
  margin-bottom: 3px;
}
.workout-form-dialog input, .workout-form-dialog select, .workout-form-dialog textarea {
  width: 100%;
  padding: 0.4em;
  border: 1px solid #e0e0e0;
  border-radius: 6px;
  font-size: 1em;
}
.form-actions {
  display: flex;
  gap: 1em;
}
.workout-list {
  margin-top: 1.8em;
}
.workout-list table {
  width: 100%;
  border-collapse: collapse;
}
.workout-list th, .workout-list td {
  padding: 0.68em;
  border-bottom: 1px solid #eaeaea;
  text-align: left;
}
.workout-list th {
  background: #f3f7f7;
}
.empty-msg {
  text-align: center;
  padding: 1.5em;
  color: #565656;
}
.loading-indicator {
  padding: 1.5em;
  text-align: center;
}
</style>
