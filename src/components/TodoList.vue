<template>
    <ul class="listTodo">
        <TodoItem v-for="todo in todoList" :key="todo.id" :todo="todo"/>
    </ul>
</template>

<script setup>
    import TodoItem from './TodoItem.vue';
    const props = defineProps ({
        todoList: Object,
    });
</script>

<style scoped>
    .listTodo {
        display: flex;
        flex-direction: column;
        width: 80%;
        margin: 0 auto;
    }
</style>