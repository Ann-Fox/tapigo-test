<script setup>
import { ref, computed, onMounted, watch } from 'vue'
import ButtonToggle from './components/ButtonToggle.vue'

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
          <p>Close</p>
        </div>
        <ul>
          <li v-for="todo in todosFilter" :key="todo.id" :class="{ 'done': todo.done }" class="task-li">
            <input type="checkbox" name="" v-model="todo.done" :id="`box-${todo.id}`" class="task-li__checkbox">
            <label :for="`box-${todo.id}`" сlass="task-li__label"> {{ todo.text }}</label>
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
  align-items: end;
}

.task-li::after {
  content: "";
  position: absolute;
  width: 100%;
  bottom: 0;
  border-bottom: #ddd5d5 solid 1px;
}

.task-li__checkbox {
  grid-area: input;

  appearance: none;
  position: relative;
  margin: 0 auto;
  width: 30px;
  height: 30px;
  background: #aec7bc;
  box-shadow: inset 0 0 5px rgb(0 0 0 / 0.2);
  border-radius: 10px;
  border: 1px solid #ffffff;
  transition: 500ms;
}

.task-li__checkbox::after {
  content: "\2714";
  position: absolute;
  color: #fff;
  top: -5px;
  left: 2px;
  width: 0px;
  height: 0px;
  font-size: 26px;
  transition: 500ms;
  overflow: hidden;
}

.task-li__checkbox:checked::after {
  width: 30px;
  height: 30px;
  transition: 500ms;
}

li>label {
  grid-area: label;
  font-weight: 300;

  text-overflow: ellipsis;
  white-space: nowrap;
  overflow: hidden;
}

.task-li__button {
  grid-area: btn;
  display: flex;
  justify-content: center;
  max-width: 30px;
  height: 100%;
  margin: 0 auto;
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

@media (max-width: 567px) {

  .header {
    font-size: 32px;
    height: 150px;
  }

  .form {
    padding: 20px 0;
  }

  .form__input {
    max-width: 300px;
  }

  .list__title {
    grid-template-columns: 2fr 1fr 1fr;
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

  .list {
    padding: 20px 0;
    gap: 20px;
  }

  .list__header {
    padding: 0 20px;
  }

  .list__title {
    padding: 0 20px;
  }

  ul {
    gap: 10px;
    padding: 0 20px;
  }
}
</style>
