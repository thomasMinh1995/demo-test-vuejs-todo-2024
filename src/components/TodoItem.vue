<template>
  <li>
    <span @click="$emit('toggle')" :class="{ 'line-through': todo.status }">
      {{ todo.status ? todo.content : editedContent }}
    </span>
    <input
      v-if="!todo.status"
      v-model="editedContent"
      @keyup.enter="update"
      placeholder="Update todo"
    />
    <button v-if="!todo.status" @click="update">Update</button>
    <button @click.stop="$emit('remove')" class="ml-2 text-red-500">Remove</button>
  </li>
</template>

<script setup>
import { ref, defineProps } from 'vue';

const emit = defineEmits(); 
const props = defineProps(['todo']);
const editedContent = ref(props.todo.content);

const update = () => {
  if (!props.todo.status) {
    // Emit the updated content to the parent component
    emit('update', editedContent.value);
  }
};
</script>

<style scoped>
.line-through {
  text-decoration: line-through;
}
</style>
