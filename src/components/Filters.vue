<script lang="ts" setup>
import type { Task, TaskFilter } from '@/types';
import FilterButton from './FilterButton.vue';

const props = defineProps<{
  tasks: Task[];
  filter: TaskFilter;
}>();

const emits = defineEmits<{
  setFilter: [filter: TaskFilter];
}>();
</script>

<template>
  <div v-if="props.tasks.length" class="button-container">
    <FilterButton
      filter="all"
      :active-filter="filter"
      @set-filter="emits('setFilter', $event)"
    />
    <FilterButton
      filter="todo"
      :active-filter="filter"
      @set-filter="emits('setFilter', $event)"
    />
    <FilterButton
      filter="done"
      :active-filter="filter"
      @set-filter="emits('setFilter', $event)"
    />
  </div>
</template>
