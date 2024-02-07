<template>
  <div
      class="max-w-md w-full p-6 bg-white bg-opacity-20 backdrop-blur-md rounded-lg"
  >
    <h1 class="text-2xl font-bold mb-4 text-center text-white">Todo List</h1>

    <FilterTodo @on-filter="doFilter" />

    <AddTodo @add-todo="addTodo"/>

    <!-- Todo list -->
    <div class="space-y-4">
      <!-- Single todo item -->
      <div
          v-for="(todoItem, index) in filteredTodo()"
          :key="index"
          :id="todoItem.id"
          ref="itemRefs"
          @dblclick="todoItem.completed = !todoItem.completed"
      >
        <div class="flex items-center justify-between" v-if="todoItem.inEdit">
          <input
              class="bg-white rounded-l-lg px-4 py-2 w-full focus:outline-none"
              v-model="todoItem.text"
          >
          <button
              class="text-green-600 bg-white bg-opacity-50 p-2 hover:text-green-500"
              @click="onUpdate(todoItem)"
          >
            <i class="bx bx-edit"></i>
          </button>
          <button
              class="text-red-500 bg-white bg-opacity-50 p-2 hover:text-red-700 rounded-r-lg"
              @click="onCancel(todoItem)"
          >
            <i class="bx bx-x-circle"></i>
          </button>
        </div>

        <div class="flex items-center justify-between bg-white bg-opacity-50 rounded-lg px-4 py-2" v-if="!todoItem.inEdit">
          <span
              class="flex-1 text-gray-800 break-all select-none"
              :class="todoItem.completed ? 'line-through' : ''"
          >
            <span @dblclick.stop="onEdit(todoItem)">{{ todoItem.text }}</span>
          </span>
          <div class="flex space-x-2">
            <!-- Edit icon -->
            <!--  @click="openEditModal(todoItem)" -->
            <button class="text-blue-500 hover:text-blue-700" title="Edit" @click="onEdit(todoItem)">
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
            <!-- completed/Uncompleted button -->
            <button
                class="text-green-500 hover:text-green-700"
                title="completed"
                @click="todoItem.completed = !todoItem.completed"
            >
              <i class="bx bx-check completed-icon" v-if="!todoItem.completed"></i>
              <i class="bx bx-check-double completed-icon" v-if="todoItem.completed"></i>
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
import FilterTodo from "@/components/Home/FilterTodo.vue"
import {ref} from "vue"

const todoList = ref([])
const itemRefs = ref(null)
const selectedTodoItem = ref(null)
const filterOption = ref('all')

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

function onUpdate(todoItem) {
  todoItem.inEdit = false
  todoItem.text = todoItem.text.trim()
}

function onEdit(todoItem) {
  todoItem.inEdit = true
  todoItem.fomerText = todoItem.text
}

function onCancel(todoItem) {
  todoItem.inEdit = false
  todoItem.text = todoItem.fomerText
}

function doFilter(filter) {
  filterOption.value = filter
}

function filteredTodo() {
  if (filterOption.value === 'all')
    return todoList.value
  if (filterOption.value === 'active')
    return todoList.value.filter(item => !item.completed)
  if (filterOption.value === 'completed')
    return todoList.value.filter(item => item.completed)
}
</script>

<style scoped>
.completed-icon {
  margin-right: -0.5rem;
}
</style>
