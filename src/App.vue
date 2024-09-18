<script setup>
import { ref, onMounted } from "vue";
import TodoItem from './components/TodoItem.vue';

const todos = ref([
  { content: "Buy groceries", status: false },
  { content: "Finish project report", status: true },
  { content: "Call the doctor", status: false },
  { content: "Read a book", status: true },
]);
const newTodo = ref("");

// Load todos from local storage on component mount
onMounted(() => {
  const storedTodos = localStorage.getItem("todos");
  if (storedTodos) {
    todos.value = JSON.parse(storedTodos);
  }
});

const saveTodos = () => {
  localStorage.setItem("todos", JSON.stringify(todos.value));
};

const addTodo = () => {
  if (newTodo.value) {
    todos.value.push({ content: newTodo.value, status: false });
    newTodo.value = "";
    saveTodos(); // Save to local storage
  }
};

const removeTodo = (index) => {
  todos.value.splice(index, 1);
  saveTodos(); // Save to local storage
};

const toggleStatus = (index) => {
  todos.value[index].status = !todos.value[index].status;
  saveTodos(); // Save to local storage
};

const updateTodo = (index, newContent) => {
  if (!todos.value[index].status) {
    todos.value[index].content = newContent;
    saveTodos(); // Save to local storage
  }
};
</script>

<template>
  <div class="max-w-md mx-auto p-6 bg-white rounded-lg shadow-md">
    <h1 class="text-3xl font-bold text-center mb-4">To-Do List</h1>
    <div class="flex mb-4">
      <input v-model="newTodo" placeholder="Add a new todo" class="flex-grow p-2 border border-gray-300 rounded-l-md" />
      <button @click="addTodo" class="bg-green-500 text-white p-2 rounded-r-md hover:bg-green-600">Add</button>
    </div>
    <ul class="divide-y divide-gray-200">
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