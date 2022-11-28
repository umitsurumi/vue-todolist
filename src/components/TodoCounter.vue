<template>
    <div class="footerTodo" v-if = "allTodo">
        <p>已完成
            <span>{{ doneTodo }}</span>
            /全部
            <span>{{ allTodo }}</span>
        </p>
        <button @click="clearItem">清除已完成任务</button>
    </div>
</template>

<script setup>
    import { computed } from 'vue';
    const props = defineProps ({
        todoList: Object,
        clearTodo: Function,
    });

    //计算属性，已完成事件数量
    const doneTodo = computed(() => {
        return props.todoList.reduce((pre,todo) => pre + (todo.done ? 1 : 0), 0)
    })
    //计算属性，全部事件数量
    const allTodo = computed(() => { return props.todoList.length });

    //清除全部已完成代办
    function clearItem() {
        if(confirm("是否清除已完成代办？")) {
            props.clearTodo();
        }
    }
</script>

<style scoped>
    .footerTodo {
        display: flex;
        padding-left: 5%;
        width: 90%;
        justify-content: space-between;
        align-items: baseline;
    }
    .footerTodo button {
        height: 30px;
        padding: 0 5px;
        background-color: #7177bd;
        border: none;
        border-radius: 10px;
        color: #fff;
    }
</style>