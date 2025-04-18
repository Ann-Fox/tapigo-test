<script setup>
import { ref } from 'vue'

const todos = ref([
  { id: 0, text: 'Learning JavaScript' },
  { id: 1, text: 'Learning Vue.js 3' },
  { id: 2, text: 'To be happy' },
])
</script>

<template>
  <header>
    <h1>TODO LIST</h1>
  </header>

  <main>
    <div>
      <input type="text">
      <button>Add task</button>
    </div>
    <ul>
      <li v-for="todo in todos">
        <input type="checkbox" name="" id="">
        {{ todo.text }}
        <button>delete task</button>
      </li>
    </ul>
    <button>show\hide all done tasks</button>
  </main>
</template>

<style scoped></style>
