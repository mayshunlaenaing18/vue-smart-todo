<template>
  <div class="min-h-screen bg-gray-100 flex items-center justify-center p-6">
    <div class="w-full max-w-md bg-white rounded-xl shadow p-6">
      <h1 class="text-2xl font-bold mb-4">My Todo App</h1>

      <!-- Input -->
      <div class="flex gap-2 mb-4">
        <input
          v-model="newTodo"
          type="text"
          placeholder="Enter todo..."
          class="flex-1 border p-2 rounded"
        />

        <button @click="addTodo" class="bg-blue-500 text-white px-4 rounded">Add</button>
      </div>

      <!-- Todo List -->
      <div class="flex gap-2 mb-4">
        <button @click="filter = 'all'" class="px-3 py-1 bg-gray-200 rounded">All</button>

        <button @click="filter = 'active'" class="px-3 py-1 bg-gray-200 rounded">Active</button>

        <button @click="filter = 'completed'" class="px-3 py-1 bg-gray-200 rounded">
          Completed
        </button>
        <p class="mb-4 text-gray-600">{{ remainingCount }} tasks remaining</p>
      </div>
      <ul>
        <li
          v-for="(todo, index) in filteredTodos"
          :key="index"
          class="p-2 border-b flex justify-between items-center"
        >
          <div class="flex items-center gap-2">
            <input type="checkbox" v-model="todo.done" />

            <span :class="{ 'line-through text-gray-400': todo.done }">
              {{ todo.text }}
            </span>
          </div>

          <button @click="removeTodo(index)" class="text-red-500">Delete</button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import { ref, watch, computed } from 'vue'

const newTodo = ref('')

const filter = ref('all')

const savedTodos = localStorage.getItem('todos')

const todos = ref(savedTodos ? JSON.parse(savedTodos) : [])

function addTodo() {
  if (newTodo.value.trim() === '') return

  todos.value.push({
    text: newTodo.value,
    done: false,
  })

  newTodo.value = ''
}

function removeTodo(index) {
  todos.value.splice(index, 1)
}

const filteredTodos = computed(() => {
  if (filter.value === 'active') {
    return todos.value.filter((todo) => !todo.done)
  }

  if (filter.value === 'completed') {
    return todos.value.filter((todo) => todo.done)
  }

  return todos.value
})

const remainingCount = computed(() => {
  return todos.value.filter((todo) => !todo.done).length
})

watch(
  todos,
  (newTodos) => {
    localStorage.setItem('todos', JSON.stringify(newTodos))
  },
  { deep: true },
)
</script>

<style></style>
