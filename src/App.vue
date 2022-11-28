<script setup>
  import NewTodo from './components/NewTodo.vue';
  import TodoList from './components/TodoList.vue';
  import TodoCounter from './components/TodoCounter.vue';
  import { reactive, watch, provide } from 'vue';

  let id = 0;
  const todoList = reactive({
      //初始化：有本地存储读取本地存储，没有则初始化一个空数组
      arr: JSON.parse(localStorage.getItem('todoList')) || []
      // arr:[
      //   { id: id++, value:'吃饭', done: true},
      //   { id: id++, value:'睡觉', done:false},
      //   { id: id++, value:'打豆豆', done:false},
      // ]
  })

  //利用localStorage进行浏览器本地保存
  watch(
    //侦听响应式对象的属性值，需要用一个返回该属性的getter函数
    () => todoList.arr, 
    (newList) => {
    localStorage.setItem('todoList', JSON.stringify(newList))
  },
    //开启深度侦听
    {deep: true}
  )


  //newTodo内容，父组件props传递函数给子组件，函数接受子组件数据
  //添加一个todo
  function addTodo(text) {
    const newTodo = {id: id++, value: text, done:false, isEditing: false};
    todoList.arr.unshift(newTodo);
  }

  //绑定todoItem勾选，传递给孙子组件，使用provide/inject方法
  const todoItemMethods = {
    //勾选一个todo
    checkTodo(id) {
      todoList.arr.forEach((todo) => {
        if(todo.id === id) {
          todo.done = !todo.done;
        }
      })
    },
    //删除一个todo
    deleteTodo(id) {
      todoList.arr = todoList.arr.filter((todo) => todo.id !== id);
    },
    editTodo(id, text) {
      todoList.arr.forEach((todo) => {
        if(todo.id === id) {
          todo.value = text;
        }
      })
    }
  };
  //provide将孙子组件需要的方法打包成对象传递
  provide('todoItemMethods', todoItemMethods);

  //删除所有已完成todo
  function clearTodo() {
    todoList.arr = todoList.arr.filter((todo) => !todo.done);
  }
</script>

<template>
  <NewTodo 
    :addTodo="addTodo"
  />
  <!--todoList从父组件（app）传递给子组件（TodoList）渲染-->
  <TodoList 
    :todoList="todoList.arr"
  />
  <!--统计数据，已完成代办数量和所有代办数量-->
  <TodoCounter 
    :todoList="todoList.arr"
    :clearTodo = "clearTodo"
  />
</template>

<style scoped>
</style>
