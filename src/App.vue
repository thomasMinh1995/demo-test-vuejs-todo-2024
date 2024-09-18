<script setup>
import { ref } from "vue";
import TodoItem from './components/TodoItem.vue';

const todos = ref([
  { content: "Buy groceries", status: false },
  { content: "Finish project report", status: true },
  { content: "Call the doctor", status: false },
  { content: "Read a book", status: true },
]);

const newTodo = ref("");

const addTodo = () => {
  if (newTodo.value) {
    todos.value.push({ content: newTodo.value, status: false });
    newTodo.value = "";
  }
};

const removeTodo = (index) => {
  todos.value.splice(index, 1);
};

const toggleStatus = (index) => {
  todos.value[index].status = !todos.value[index].status;
};

const updateTodo = (index, newContent) => {
  if (!todos.value[index].status) {
    todos.value[index].content = newContent;
  }
};

</script>

<template>
  <div>
    <h1 class="text-3xl font-bold underline">To-Do List</h1>
    <input v-model="newTodo" placeholder="Add a new todo" />
    <button @click="addTodo">Add</button>
    <ul>
      <TodoItem
        v-for="(todo, index) in todos"
        :key="index"
        :todo="todo"
        @toggle="toggleStatus(index)"
        @update="updateTodo(index, $event)"
        @remove="removeTodo(index)"
      />
    </ul>
  </div>
</template>
