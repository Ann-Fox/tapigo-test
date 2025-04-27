<script setup>
const props = defineProps({
    show: {
        type: Boolean
    },
    task: {
        type: Object,
        default: () => ({})
    }
})
</script>

<template>
    <Transition name="modal">
        <div v-if="show" class="modal-mask">
            <div class="modal-container">
                <div class="modal-header">
                    Do you really want to delete the task? {{ task.id - task.text }}
                </div>
                <div class="modal-body">
                    <button @click="$emit('delete')" class="btn btn-delete">Yes</button>
                    <button @click="$emit('close')" class="btn btn-close">No</button>
                </div>
            </div>
        </div>
    </Transition>
</template>

<style>
.modal-mask {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    transition: opasity 0.3s ease;
    display: flex;
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