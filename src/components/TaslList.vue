<script setup lang="ts">
import { computed } from 'vue';
import type { Task } from '../types';

const props = defineProps<{
  tasks: Task[];
}>();

const emits = defineEmits<{
  toggleDone: [id: string];
  removeTask: [id: string];
}>();

const totalDone = computed(() =>
  props.tasks.reduce((total, task) => (task.done ? total + 1 : total), 0),
);
</script>

<template>
  <div class="task-container">
    <h3 v-if="!props.tasks.length" class="empty-state">
      You have not any tasks yet
    </h3>

    <template v-else>
      <p class="task-count">
        Task done {{ totalDone }} / {{ props.tasks.length }}
      </p>

      <TransitionGroup name="task-list" tag="div" class="task-list">
        <article class="task" v-for="task in props.tasks" :key="task.id">
          <div class="task-text">
            <input
              @input="emits('toggleDone', task.id)"
              :checked="task.done"
              type="checkbox"
            />
            <p :class="{ done: task.done }">{{ task.title }}</p>
          </div>
          <button @click="emits('removeTask', task.id)" class="outline">
            Delete
          </button>
        </article>
      </TransitionGroup>
    </template>
  </div>
</template>

<style scoped>
.task-container {
  padding: 20px 0;
}

.task-list-enter-active,
.task-list-leave-active {
  transition: all 0.5s ease;
}

.task-list-enter-from,
.task-list-leave-to {
  opacity: 0;
  transform: translateX(300px);
}

.done {
  text-decoration: line-through;
}

.empty-state {
  text-align: center;
}

.task-count {
  color: var(--primary);
}

.task {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.task-text {
  display: flex;
  align-items: center;
}

.task p {
  margin-bottom: 0;
}

ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

li {
  padding: 0.5rem;
}
</style>
