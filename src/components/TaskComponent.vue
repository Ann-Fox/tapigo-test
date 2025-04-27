<script setup>
import { ref } from 'vue';

const props = defineProps({
    task: {
        type: Object,
        default: () => ({})
    },
})

const showModal = ref(false)
</script>

<template>
    <li class="task">
        <input type="checkbox" name="" v-model="task.done" :id="`box-${task.id}`" class="task__checkbox">
        <label :for="`box-${task.id}`" сlass="task__label" :class="{ 'done': task.done }"> {{ task.text }}</label>
        <button class="task__button" @click="showModal = !showModal"><img src="./icons/delete_24.svg"></button>
    </li>

    <Transition name="modal">
        <div v-if="showModal" class="modal-mask">
            <div class="modal-container">
                <div class="modal-header">
                    Do you really want to delete the task? {{ task.text }}
                </div>
                <div class="modal-body">
                    <button class="btn btn-delete" @click="$emit('remove-task')">Yes</button>
                    <button class="btn btn-close" @click="showModal = false">No</button>
                </div>
            </div>
        </div>
    </Transition>
</template>

<style>
.done {
    text-decoration: line-through;
}

.task {
    position: relative;
    padding: 10px 0;
    display: grid;
    grid-template-columns: 2fr 1fr 1fr;
    grid-template-areas:
        "label input btn";
    align-items: end;
}

.task::after {
    content: "";
    position: absolute;
    width: 100%;
    bottom: 0;
    border-bottom: #ddd5d5 solid 1px;
}

.task__checkbox {
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

.task__checkbox::after {
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

.task__checkbox:checked::after {
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

.task__button {
    grid-area: btn;
    display: flex;
    justify-content: center;
    max-width: 30px;
    height: 100%;
    margin: 0 auto;
    border-radius: 50%;

    background: -webkitnear-gradient(159deg, #ffffff, #fa0b23);
    /* Chrome 10-25, Safari 5.1-6 */
    background: linear-gradient(159deg, #ffffff, #fa0b23);
    /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
}

.modal-mask {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.6);
    transition: opasity 0.3s ease;
    display: flex;
    z-index: 10;
}

.modal-container {
    width: 450px;
    margin: auto;
    padding: 30px;
    background-color: #fff;
    border-radius: 8px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
}

.modal-header {
    margin-bottom: 20px;
}

.modal-body {
    display: flex;
    gap: 10px;
}

.btn-close {
    background-color: #13d83e;
}

.btn-delete {
    background-color: #ff3d51;
}
</style>