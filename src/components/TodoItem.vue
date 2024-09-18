<template>
  <li class="flex items-center justify-between py-2">
    <span 
      @click="$emit('toggle')" 
      :class="{'line-through text-gray-400': todo.status, 'cursor-pointer': !todo.status }"
      class="flex-grow"
    >
      {{ todo.status ? todo.content : editedContent }}
    </span>
    <input
      v-if="!todo.status"
      v-model="editedContent"
      @keyup.enter="update"
      placeholder="Update todo"
      class="ml-2 p-1 border border-gray-300 rounded"
    />
    <button v-if="!todo.status" @click="update" class="ml-2 bg-blue-500 text-white px-2 rounded hover:bg-blue-600">Update</button>
    <button @click.stop="$emit('remove')" class="ml-2 bg-red-500 text-white px-2 rounded hover:bg-red-600">Remove</button>
  </li>
</template>

<script setup>
import { ref, defineProps } from 'vue';

const emit = defineEmits(); 
const props = defineProps(['todo']);
const editedContent = ref(props.todo.content);

const update = () => {
  if (!props.todo.status) {
    emit('update', editedContent.value);
  }
};
</script>
