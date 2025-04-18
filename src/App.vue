<script setup>
import { ref } from 'vue'

let id = 0

const newTodo = ref('')
const todos = ref([
  { id: id++, text: 'Learning JavaScript' },
  { id: id++, text: 'Learning Vue.js 3' },
  { id: id++, text: 'To be happy' },
])

function addTask() {
  todos.value.push({ id: id++, text: newTodo.value })
  newTodo.value = ''
}

function removeTask(todo) {
  todos.value = todos.value.filter((t) => t !== todo)
}
</script>

<template>
  <header>
    <h1>TODO LIST</h1>
  </header>

  <main>
    <div>
      <input type="text" v-model="newTodo" placeholder="write a task" required>
      <button @click="addTask">Add task</button>
    </div>
    <ul>
      <li v-for="todo in todos" :key="todo.id">
        <input type="checkbox" name="" id="">
        {{ todo.text }}
        <button @click="removeTask(todo)">del</button>
      </li>
    </ul>
    <button>show\hide all done tasks</button>
  </main>
</template>

<style scoped></style>
