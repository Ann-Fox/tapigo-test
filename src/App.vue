<script setup>
import { ref, computed } from 'vue'
import ButtonToggle from './components/ButtonToggle.vue'

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
          <p>Close</p>
        </div>
        <ul>
          <li v-for="todo in dotosFilter" :key="todo.id" :class="{ 'done': todo.done }" class="task-li">
            <input type="checkbox" name="" v-model="todo.done" :id="`box-${todo.id}`" class="task-li__input">
            <label :for="`box-${todo.id}`" lass="task-li__label"> {{ todo.text }}</label>
            <button class="task-li__button" @click="removeTask(todo)"><img
                src="./components/icons/delete_24.svg"></button>
          </li>
        </ul>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  background-color: #f7f7ff;
  width: 100%;
  max-width: 800px;
  margin: 0 auto;
  padding-bottom: 50px;
  border-radius: 20px;
}

.header {
  text-align: center;
  font-size: 36px;
  color: #fff;
  font-weight: 300;

  height: 200px;
  border-radius: 20px 20px 0 0;
  background: -webkit-linear-gradient(90deg, #604486, #59538d, #516395);
  /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(90deg, #604486, #59538d, #516395);
  /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
}

.form {
  background-color: #fff;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 30px 0;
  border-radius: 20px;
  gap: 20px;

  width: 90%;
  margin-inline: auto;
  /* inset-inline: 0; */
  inset-block-start: 40%;
  transform: translateY(-70%);

  -webkit-box-shadow: 0px 1px 42px -11px rgba(0, 0, 0, 0.2);
  -moz-box-shadow: 0px 1px 42px -11px rgba(0, 0, 0, 0.2);
  box-shadow: 0px 1px 42px -11px rgba(0, 0, 0, 0.2);
}

.form__input {
  width: 100%;
  max-width: 400px;
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

  -webkit-box-shadow: 0px 1px 42px -11px rgba(0, 0, 0, 0.2);
  -moz-box-shadow: 0px 1px 42px -11px rgba(0, 0, 0, 0.2);
  box-shadow: 0px 1px 42px -11px rgba(0, 0, 0, 0.2);
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
  grid-template-columns: 3fr 1fr 1fr;
  margin-bottom: 20px;
  background-color: #f9f9f9;
  color: #d7d1cb;
  padding: 0 30px;
  font-weight: 300;
}

ul {
  padding: 0 30px;
  max-height: 500px;
  overflow-y: auto;

  display: flex;
  flex-direction: column;
  gap: 20px;
}

.task-li {
  position: relative;
  padding: 10px 0;
  display: grid;
  grid-template-columns: 2fr 1fr 1fr;
  grid-template-areas:
    "label input btn";
}

.task-li::after {
  content: "";
  position: absolute;
  width: 100%;
  bottom: 0;
  border-bottom: #ddd5d5 solid 1px;
}

.task-li__input {
  grid-area: input;
}

.task-li__label {
  grid-area: label;
  font-weight: 300;
}

.task-li__button {
  grid-area: btn;
  display: flex;
  justify-content: center;
  max-width: 30px;
  height: 100%;
  border-radius: 50%;

  background: -webkit-linear-gradient(159deg, #ffffff, #fa0b23);
  /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(159deg, #ffffff, #fa0b23);
  /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
}

.button__remove_img svg {
  color: #000;
}


.done {
  text-decoration: line-through;

}
</style>
