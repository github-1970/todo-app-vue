<template>
  <h1 class="text-2xl font-bold mb-4 text-center text-white">Todo List</h1>
  <AddTodo @add-todo="addTodo" />

  <!-- Todo list -->
    <div class="space-y-4">
      <!-- Single todo item -->
      <div class="flex items-center justify-between bg-white bg-opacity-50 rounded-lg px-4 py-2" v-for="(todoItem, index) in todoList" :key="index" :id="todoItem.id">
        <span class="flex-1 text-gray-800">{{ todoItem.text }}</span>
        <div class="flex space-x-2">
          <!-- Edit icon -->
          <button class="text-blue-500 hover:text-blue-700" title="Edit">
            <i class='bx bx-edit-alt'></i>
          </button>
          <!-- Delete icon -->
          <button class="text-red-500 hover:text-red-700" title="Delete">
            <i class='bx bx-trash'></i>
          </button>
          <!-- Success/Unsuccess button -->
          <button class="text-green-500 hover:text-green-700" title="Success">
            <i class='bx bx-check'></i>
          </button>
        </div>
      </div>
    </div>
</template>

<script setup>
import AddTodo from '@/components/AddTodo.vue';
import { ref } from 'vue'

const todoList = ref([]);

function addTodo(data) {
  todoList.value.push(data)
}
</script>