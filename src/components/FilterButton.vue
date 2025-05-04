<script lang="ts" setup>
import type { TaskFilter } from '@/types';

const props = defineProps<{
  filter: TaskFilter;
  activeFilter: TaskFilter;
}>();

const emits = defineEmits<{
  setFilter: [filter: TaskFilter];
}>();
</script>

<template>
  <button
    :class="[activeFilter === filter ? 'secondary' : 'outline']"
    @click="emits('setFilter', props.filter)"
  >
    {{ props.filter }}
  </button>
</template>

<style scoped>
button {
  text-transform: capitalize;
}
</style>
