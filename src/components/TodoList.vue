<template>
  <h1 class="text-2xl font-bold mb-4 text-center text-white">Todo List</h1>
  <AddTodo @add-todo="addTodo" />

  <!-- Todo list -->
  <div class="space-y-4">
    <!-- Single todo item -->
    <div
      class="flex items-center justify-between bg-white bg-opacity-50 rounded-lg px-4 py-2"
      v-for="(todoItem, index) in todoList"
      :key="index"
      :id="todoItem.id"
      ref="itemRefs"
    >
      <span class="flex-1 text-gray-800">{{ todoItem.text }}</span>
      <div class="flex space-x-2">
        <!-- Edit icon -->
        <!--  @click="openEditModal(todoItem)" -->
        <button class="text-blue-500 hover:text-blue-700" title="Edit">
          <i class="bx bx-edit-alt"></i>
        </button>
        <!-- Delete icon -->
        <!-- @click="openDeleteModal(todoItem)" -->
        <button class="text-red-500 hover:text-red-700" title="Delete">
          <i class="bx bx-trash"></i>
        </button>
        <!-- Success/Unsuccess button -->
        <button
          class="text-green-500 hover:text-green-700"
          title="Success"
          @click="toggleSuccess(todoItem)"
        >
          <i class="bx bx-check success-icon !hidden"></i>
          <i class="bx bx-check"></i>
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import AddTodo from "@/components/AddTodo.vue";
import { ref } from "vue";
const todoList = ref([]);
const itemRefs = ref(null);

function addTodo(data) {
  todoList.value.push(data);
}

function toggleSuccess(todoItem) {
  itemRefs.value.forEach((itemRef) => {
    if (itemRef.id == todoItem.id) {
      itemRef.classList.toggle("line-through");
      itemRef.querySelector('i.success-icon').classList.toggle('!hidden')
    }
  });
}
</script>

<style scoped>
.success-icon {
  margin-right: -.5rem;
}
</style>
