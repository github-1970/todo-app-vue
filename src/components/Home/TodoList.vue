<template>
  <div
      class="max-w-md w-full p-6 bg-white bg-opacity-20 backdrop-blur-md rounded-lg"
  >
    <h1 class="text-2xl font-bold mb-4 text-center text-white">Todo List</h1>
    <AddTodo @add-todo="addTodo"/>

    <!-- Todo list -->
    <div class="space-y-4">
      <!-- Single todo item -->
      <div
          v-for="(todoItem, index) in todoList"
          :key="index"
          :id="todoItem.id"
          ref="itemRefs"
          @dblclick="todoItem.success = !todoItem.success"
      >
        <div class="flex items-center justify-between" v-if="todoItem.inEdit">
          <input
              class="bg-white bg-opacity-50 rounded-l-lg px-4 py-2 w-full"
              v-model="todoItem.text"
              autofocus
          >
          <button
              class="bg-green-500 text-white px-4 py-2 rounded-r-lg hover:bg-green-600 focus:outline-none focus:ring-2 ring-inset focus:ring-green-500"
              @click="onEdit(todoItem)"
          >
            Edit
          </button>
        </div>

        <div class="flex items-center justify-between bg-white bg-opacity-50 rounded-lg px-4 py-2" v-if="!todoItem.inEdit">
          <span
              class="flex-1 text-gray-800"
              :class="todoItem.success ? 'line-through' : ''"
          >
            <span @dblclick.stop="todoItem.inEdit = true">{{ todoItem.text }}</span>
          </span>
          <div class="flex space-x-2">
            <!-- Edit icon -->
            <!--  @click="openEditModal(todoItem)" -->
            <button class="text-blue-500 hover:text-blue-700" title="Edit" @click="todoItem.inEdit = true">
              <i class="bx bx-edit-alt"></i>
            </button>
            <!-- Delete icon -->
            <button
                class="text-red-500 hover:text-red-700"
                title="Delete"
                @click="openDeleteModal(todoItem)"
            >
              <i class="bx bx-trash"></i>
            </button>
            <!-- Success/Unsuccess button -->
            <button
                class="text-green-500 hover:text-green-700"
                title="Success"
                @click="todoItem.success = !todoItem.success"
            >
              <i class="bx bx-check success-icon" v-if="todoItem.success"></i>
              <i class="bx bx-check"></i>
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- Modal for delete confirmation -->
  <ConfirmModal
      v-if="selectedTodoItem"
      :message="'Are you sure you want to delete this todo item?'"
      @confirm="deleteTodoItem"
      @close-delete-modal="closeDeleteModal"/>
</template>

<script setup>
import AddTodo from "@/components/Home/AddTodo.vue"
import ConfirmModal from "@/components/ConfirmModal.vue"
import {ref} from "vue"

const todoList = ref([])
const itemRefs = ref(null)
const selectedTodoItem = ref(null)

function addTodo(data) {
  todoList.value.unshift(data)
}

function openDeleteModal(todoItem) {
  selectedTodoItem.value = todoItem
}

function closeDeleteModal() {
  selectedTodoItem.value = null
}

function deleteTodoItem() {
  todoList.value = todoList.value.filter(
      (item) => item.id !== selectedTodoItem.value.id
  )
  closeDeleteModal()
}

function onEdit(todoItem) {
  todoItem.inEdit = false
  todoItem.text = todoItem.text.trim()
}
</script>

<style scoped>
.success-icon {
  margin-right: -0.5rem;
}
</style>
