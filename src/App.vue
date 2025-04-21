<script setup>
import { ref, computed } from 'vue'

let id = 0

const completed = ref(false)
const newTodo = ref('')
const todos = ref([
  { id: id++, text: 'Learning JavaScript', done: true },
  { id: id++, text: 'Learning Vue.js 3', done: true },
  { id: id++, text: 'To be happy', done: false },
])

const dotosFilter = computed(() => {
  return completed.value ? todos.value.filter((t) => !t.done) : todos.value
})

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
    <form @submit.prevent="addTask">
      <input type="text" required v-model="newTodo" placeholder="write a task">
      <button>Add task</button>
    </form>
    <ul>
      <li v-for="todo in dotosFilter" :key="todo.id" :class="{ 'done': todo.done }">
        <input type="checkbox" name="" v-model="todo.done" :id="`box-${todo.id}`">
        <label :for="`box-${todo.id}`"> {{ todo.text }}</label>
        <button @click="removeTask(todo)">del</button>
      </li>
    </ul>
    <button @click="completed = !completed">{{ completed ? 'show all tasks' : 'hide done tasks' }}</button>
  </main>
</template>

<style scoped>
.done {
  text-decoration: line-through;
}
</style>
