<script setup>
import { ref, computed, onMounted, watch } from 'vue'
import ButtonToggle from './components/ButtonToggle.vue'
import TaskComponent from './components/TaskComponent.vue'
let id = 0

const completed = ref()
const newTodo = ref('')
const todos = ref()

onMounted(() => {
  const storeData = localStorage.getItem('todos')
  const completedData = localStorage.getItem('completed')

  if (completedData) {
    completed.value = JSON.parse(completedData)
  } else {
    completed.value = false
  }

  if (storeData) {
    todos.value = JSON.parse(storeData)
  } else {
    const url = import.meta.env.BASE_URL + 'tasks.json'

    fetch(url).then(response => {
      response.json().then(todosJson => {
        todos.value = todosJson
      })
    })
  }
})

watch(completed, (newValue) => {
  localStorage.setItem('completed', JSON.stringify(newValue))
})

watch(todos, (newValue) => {
  localStorage.setItem('todos', JSON.stringify(newValue))

  const todosFilter = computed(() => {
    return completed.value ? todos.value.filter((t) => !t.done) : todos.value
  })
},
  { deep: true })

const todosFilter = computed(() => {
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
  <main>
    <div class="header">
      <h1>TODO list</h1>
    </div>

    <form class="form" @submit.prevent="addTask">
      <input class="form__input" type="text" required v-model="newTodo" placeholder="What would you like to do?">
      <button class="form__button">Add task</button>
    </form>

    <div class="list">
      <div class="list__header">
        <h3>TODO List</h3>
        <ButtonToggle @click="completed = !completed" :completed="completed"></ButtonToggle>
      </div>

      <div class="list__block">
        <div class="list__title">
          <p>List</p>
          <p>Status</p>
          <p>Edit</p>
          <p>Close</p>
        </div>
        <ul>
          <TaskComponent v-for="todo in todosFilter" :key="todo.id" :task="todo" @remove-task="removeTask(todo)">
          </TaskComponent>
        </ul>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  height: 100%;
  background-color: #f7f7ff;
}

.header {
  height: 150px;
  text-align: center;
  font-size: 36px;
  color: #fff;
  font-weight: 300;
  background: -webkit-linear-gradient(90deg, #604486, #59538d, #516395);
  /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(90deg, #604486, #59538d, #516395);
  /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
}

.form {
  background-color: #fff;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 30px;
  border-radius: 20px;
  gap: 20px;

  width: 90%;
  margin-inline: auto;
  inset-block-start: 40%;
  transform: translateY(-70%);
}

.form__input {
  width: 100%;
  max-width: 600px;
  border: 0;
  border-bottom: 2px solid #000;
  outline: 0;
}

.form__input:focus {
  border-image: linear-gradient(to right, #11998e, #38ef7d);
  border-image-slice: 1;
}

.form__button {
  background-color: #ed6a5a;
  max-width: 150px;
  text-transform: uppercase;
}

.list {
  display: flex;
  flex-direction: column;
  gap: 30px;
  width: 90%;
  margin: -60px auto 0 auto;
  padding: 30px 0;
  background-color: #ffffff;
  border-radius: 20px;
}

.list__header {
  display: grid;
  grid-template-columns: 55% 45%;
  align-items: end;
  padding: 0 30px;
}

h3 {
  font-weight: 600;
}

.list__title {
  display: grid;
  grid-template-columns: 2fr 0.5fr 0.5fr 0.5fr;
  margin-bottom: 20px;
  background-color: #f9f9f9;
  color: #d7d1cb;
  padding: 0 30px;
  font-weight: 300;
}

.list__title>p:not(:first-child) {
  text-align: center;
}

ul {
  padding: 0 30px;
  max-height: calc(100vh - 400px);
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  gap: 10px;
}

@media (max-width: 768px) {
  .form {
    padding: 20px;
    transform: translateY(-125%);
  }

  .list {
    gap: 20px;
    padding: 20px 0;
  }

  .list__header {
    padding: 0 20px;
  }

  .list__title {
    margin-bottom: 10px;
    padding: 0 20px;
  }

  ul {
    padding: 0 20px;
    max-height: calc(100vh - 325px);
    gap: 10px;
  }
}

@media (max-width: 567px) {

  .header {
    font-size: 32px;
    height: 150px;
  }

  .form {
    padding: 10px 0;
    flex-direction: column;
    transform: translateY(-90%);
    gap: 10px;
  }

  .form__input {
    max-width: 300px;
  }

  .list__title {
    grid-template-columns: 2fr 1fr 1fr 1fr;
  }

  .list__title>p:not(:first-child) {
    text-align: center;
  }
}

@media (max-width: 425px) {
  .header {
    font-size: 28px;
    height: 130px;
  }

  .form__input {
    max-width: 200px;
  }

  ul {
    max-height: calc(100vh - 310px);
  }
}
</style>
