<template>
    <li class="itemTodo">
        <div v-show="!todo.isEditing">
            <input type="checkbox" :checked="todo.done" @change="changeItem(todo.id)"> 
            <span> {{todo.value}} </span>
        </div>
        <span v-show="todo.isEditing"><input v-model="text" :id="todo.id" @blur = "endEdit(todo)"/></span>
        <div class="editor">
            <button v-show="!todo.isEditing" @click="editItem(todo)">编辑</button>
            <button @click="deleteItem(todo.id)">删除</button>
        </div>
        
    </li>
</template>

<script setup>
    import { ref, inject, nextTick } from 'vue';
    const text = ref("");
    const props = defineProps({
        todo: Object
    });

    //inject使用爷组件的方法，方法用对象包裹
    const methods = inject('todoItemMethods');
    
    //改变勾选状态和删除代办（调用爷组件的方法）
    function changeItem(id) {
        methods.checkTodo(id);
    }
    function deleteItem(id) {
        if(confirm("是否确认删除代办？")) {
            methods.deleteTodo(id);
        }
    }
    function editItem(todo) {
        text.value = todo.value;
        todo.isEditing = true;
        //nextTick在下次DOM更新循环结束之后执行延迟回调，根据id获取dom
        nextTick(() => document.getElementById(String(todo.id)).focus())
        //setTimeout(() => document.getElementById(String(todo.id)).focus())
    }
    function endEdit(todo) {
        methods.editTodo(todo.id, text.value);
        todo.isEditing = false;
    }
</script>

<style scoped>
    .itemTodo {
        display: flex;
        justify-content: space-between;
        width: 100%;
        height: 30px;
        margin-bottom: 20px;
        border-bottom: 1px solid rgb(233, 64, 182);
    }
    .itemTodo input {
        margin-right: 5px;
    }
    .editor {
        display: flex;;
        width: 50%;
        justify-content: flex-end;
    }
    .editor button {
        display: none;
        width: 30%;
        margin-left: 5px;
        border: none;
        color: #fff;
        background-color: #e95d5d;
    }
    .itemTodo:hover button {
        display: block;
    }
</style>