<script setup>
import TaskList from './components/TaskList.vue';
</script>

<template>
  <header>
    <h1>Task list Tapigo</h1>
  </header>

  <main>
    <TaskList />
  </main>
</template>

<style scoped>
header {
  display: flex;
  justify-content: center;
}
</style>
