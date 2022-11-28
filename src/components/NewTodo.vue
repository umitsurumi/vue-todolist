<template>
    <div class="inputTodo">
        <input type="text" v-model="newTodo" @keyup.enter="add" placeholder="输入代办事项，按回车键确认">
        <button @click="add">添加</button>
    </div>
</template>

<script setup>
    import { ref } from 'vue';
    //props接受父组件的函数
    const props = defineProps ({
        addTodo: Function
    });
    const newTodo = ref('')
    function add(){
        if(newTodo.value === "") {
            alert("输入不能为空！")
            return;
        }
        let text = newTodo.value;
        //利用父组件传递的函数，实现数据从子组件传递给父组件
        props.addTodo(text);
        newTodo.value = "";
    }
</script>

<style scoped>
    .inputTodo {
        display: flex;
        width: 90%;
        margin: 0 auto 30px;
    }
    .inputTodo input {
        width: 100%;
        padding: 5px 10px;
        border: none;
    }
    .inputTodo input:focus {
        outline: 2px solid skyblue;
    }
    .inputTodo button {
        width: 15%;
        border: none;
        color: #fff;
        background-color: #e95d5d;
        margin-left: calc(3px - 15%);
    }
</style>