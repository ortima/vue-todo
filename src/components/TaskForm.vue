<script setup lang="ts">
import { ref } from 'vue';

const newTask = ref('');
const error = ref();

const emit = defineEmits<{
  addTask: [newTask: string];
}>();

function formSubmitted() {
  if (newTask.value.trim()) {
    emit('addTask', newTask.value.trim());
    newTask.value = '';
  } else {
    error.value = 'Task cannot be empty!';
  }
}
</script>

<template>
  <form @submit.prevent="formSubmitted">
    <label for="newTask">New Task</label>
    <input
      @input="error = ''"
      :aria-invalid="!!error || undefined"
      type="text"
      name="newTask"
      v-model="newTask"
    />
    <small v-if="error" id="invalid-helper">{{ error }}</small>

    <div class="button-container">
      <button type="submit">Add</button>
    </div>
  </form>
</template>
