<script setup>
import { ref, onMounted, watch } from 'vue';

const items = ref();

onMounted(() => {
  const storeData = localStorage.getItem('items');

  if (storeData) {
    items.value = JSON.parse(storeData);
  } else {
    const url = '/tapigo-test/tasks.json';

    fetch(url).then(response => {
      response.json().then(itemsJson => {
        items.value = itemsJson;
      })
    });
  }
});

watch(items, (newValue) => {
  localStorage.setItem("items", JSON.stringify(newValue));
},
  { deep: true }
);

</script>

<template>
  <div>
    <ul>
      <li v-for="item in items" class="item" :key="item.id">
        <input class="checkbox-input" type="checkbox" v-model="item.done" :id="`box-${item.id}`">
        <label class="checkbox-label" :class="{ 'completed': item.done }" :for="`box-${item.id}`">{{ item.title }}</label>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.completed {
  text-decoration: line-through;
}

.item {
  margin-top: 5px;
  width: fit-content;
  display: flex;
  justify-content: center;
  align-items: center;
}

.checkbox-label {
  padding-left: 5px;
}

.checkbox-input {
  appearance: none;
  position: relative;
  width: 30px;
  height: 30px;
  background: #16d686;
  box-shadow: inset 0 0 5px rgb(0 0 0 / 0.2);
  border-radius: 10px;
  border: 1px solid #FFFFFF;
  transition: 500ms;
}

.checkbox-input:checked::after {
  width: 30px;
  height: 30px;
  transition: 500ms;
}

.checkbox-input::after {
  content: "";
  position: absolute;
  width: 0px;
  height: 0px;
  font-size: 30px;
  background-image: url("./icons/check.svg");
  background-repeat: no-repeat;
  transition: 500ms;
}
</style>
