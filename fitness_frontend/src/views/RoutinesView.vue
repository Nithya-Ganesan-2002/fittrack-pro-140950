<script setup lang="ts">
import { ref } from 'vue';

interface Routine {
  id: number;
  name: string;
  days: string[];
  exercises: string[];
}

const routines = ref<Routine[]>([]);
const showForm = ref(false);

const form = ref<{ name: string; days: string[]; exercises: string }>({
  name: '',
  days: [],
  exercises: '',
});

const weekDays = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'];

function loadRoutines() {
  routines.value = [
    {
      id: 1,
      name: 'Push/Pull/Legs',
      days: ['Mon', 'Wed', 'Fri'],
      exercises: ['Bench Press', 'Row', 'Squat'],
    },
    {
      id: 2,
      name: 'Cardio Days',
      days: ['Tue', 'Thu'],
      exercises: ['Run', 'Bike'],
    },
  ];
}

// PUBLIC_INTERFACE
function addRoutine() {
  routines.value.push({
    id: Date.now(),
    name: form.value.name,
    days: [...form.value.days],
    exercises: form.value.exercises.split(',').map(e => e.trim()).filter(Boolean),
  });
  showForm.value = false;
  form.value = { name: '', days: [], exercises: '' };
}

function openForm() {
  showForm.value = true;
}

function closeForm() {
  showForm.value = false;
}

loadRoutines();
</script>

<template>
  <div class="routines-view">
    <header>
      <h2>Exercise Routines</h2>
      <button @click="openForm" class="primary">Add Routine</button>
    </header>
    <div v-if="showForm" class="routine-form-dialog">
      <form @submit.prevent="addRoutine">
        <h3>New Routine</h3>
        <div>
          <label>Routine Name</label>
          <input v-model="form.name" required maxlength="60" />
        </div>
        <div>
          <label>Days</label>
          <div class="weekdays">
            <label v-for="d in weekDays" :key="d">
              <input 
                type="checkbox" 
                :value="d" 
                v-model="form.days"
              /> {{ d }}
            </label>
          </div>
        </div>
        <div>
          <label>Exercises (comma separated)</label>
          <input v-model="form.exercises" required />
        </div>
        <div class="form-actions">
          <button type="submit" class="primary">Save</button>
          <button type="button" @click="closeForm">Cancel</button>
        </div>
      </form>
    </div>
    <div v-if="routines.length > 0" class="routine-list">
      <div class="routine-card" v-for="r in routines" :key="r.id">
        <div class="routine-name">{{ r.name }}</div>
        <div class="routine-days">Days: <span>{{ r.days.join(', ') }}</span></div>
        <div class="routine-exercises">Exercises: <span>{{ r.exercises.join(', ') }}</span></div>
      </div>
    </div>
    <div v-else class="empty-msg">No routines defined.</div>
  </div>
</template>

<style scoped>
.routines-view { max-width: 700px; margin: 0 auto; }
header { display: flex; justify-content: space-between; align-items: center; }
button.primary {
  background: var(--primary, #4CAF50);
  color: #fff; border: none;
  padding: 0.5em 1em; border-radius: 6px; cursor: pointer; font-weight: 500;
}
.routine-form-dialog {
  background: #fff; border-radius: 8px;
  box-shadow: 0 2px 18px 0 rgba(44,62,80,0.10);
  padding: 1.2em 1.5em; margin: 1em 0;
}
.routine-form-dialog form > div { margin-bottom: 0.7em; }
.routine-form-dialog label { display: block; font-size: 1.02em; font-weight: 500; margin-bottom: 3px; }
.routine-form-dialog input[type="text"], .routine-form-dialog input[type="checkbox"] { margin-right: 0.5em; }
.routine-form-dialog input[type="text"] {
  width: 100%; padding: 0.4em; border-radius: 6px; border: 1px solid #e0e0e0; font-size: 1em;
}
.weekdays { display: flex; gap: 0.5em; flex-wrap: wrap; }
.form-actions { display: flex; gap: 1em; }
.routine-list { margin-top: 1.8em; }
.routine-card {
  background: #f2fff3;
  border-radius: 10px; box-shadow: 0 1px 7px 0 rgba(44,62,80,0.05);
  padding: 1.1em 1em; margin-bottom: 1em;
}
.routine-name { font-size: 1.2em; color: var(--secondary, #2E7D32); font-weight: 600;}
.routine-days, .routine-exercises { font-size: 1em; margin-top: 0.3em;}
.empty-msg { text-align: center; padding: 1.2em;}
</style>
