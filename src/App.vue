<script setup lang="ts">
import { computed, ref } from 'vue';
import TaskForm from './components/TaskForm.vue';
import type { Task, TaskFilter } from './types';
import TaslList from './components/TaslList.vue';
import Filters from './components/Filters.vue';

const tasks = ref<Task[]>([]);
const filter = ref<TaskFilter>('all');

const filteredTasks = computed(() => {
  switch (filter.value) {
    case 'all':
      return tasks.value;
    case 'done':
      return tasks.value.filter((task) => task.done);
    case 'todo':
      return tasks.value.filter((task) => !task.done);
    default:
      return tasks.value;
  }
});

function setFilter(value: TaskFilter) {
  filter.value = value;
}

function addTask(newTask: string) {
  tasks.value.push({
    id: crypto.randomUUID(),
    title: newTask,
    done: false,
  });
}

function toggleDone(id: string) {
  const task = tasks.value.find((task) => task.id === id);

  if (task) {
    task.done = !task.done;
  }
}

function removeTask(id: string) {
  tasks.value = tasks.value.filter((task) => task.id !== id);
}
</script>

<template>
  <main>
    <h1 class="title">Simple Todo Vue</h1>

    <TaskForm @add-task="addTask" />

    <Filters :tasks="tasks" :filter="filter" @set-filter="setFilter" />

    <TaslList
      :tasks="filteredTasks"
      @toggle-done="toggleDone"
      @remove-task="removeTask"
    />
  </main>
</template>

<style scoped>
main {
  max-width: 800px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  padding: 80px 0;
}

.title {
  text-align: center;
}

.button-container {
  display: flex;
  justify-content: end;
  gap: 8px;
}
</style>
