<template>
  <h1 class="text-2xl font-bold mb-4 text-center text-white">Todo List</h1>
  <AddTodo @add-todo="addTodo" />

  <li v-for="(todoItem, index) in todoList" :key="index" :id="todoItem.id">{{ todoItem.text }}</li>
</template>

<script setup>
import AddTodo from '@/components/AddTodo.vue';
import { ref } from 'vue'

const todoList = ref([]);

function addTodo(data) {
  todoList.value.push(data)
}
</script>