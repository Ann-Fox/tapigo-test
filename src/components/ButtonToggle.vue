<template>
    <button class="button__toggle" :class="{ 'hide': props.completed }">
        {{ props.completed ? 'show tasks' : 'hide tasks' }}</button>
</template>

<script setup>
const props = defineProps({
    completed: { type: Boolean, default: false },
})
</script>

<style>
.button__toggle {
    background-color: #ff3d51;
    max-width: 200px;
    font-size: 16px;
}

.hide {
    background-color: #aec7bc;
}

@media (max-width: 567px) {

    .button__toggle {
        padding: 5px;
    }
}

@media (max-width: 425px) {
    .button__toggle {
        font-size: 14px;

    }
}
</style>