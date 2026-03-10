<script setup>
import { ref, computed, onMounted, watch } from 'vue'

const newTask = ref('')
const tasks = ref([])
const currentPage = ref(1)
const itemsPerPage = 5

onMounted(() => {
  const savedTasks = localStorage.getItem('my-todo-tasks')
  if (savedTasks) {
    tasks.value = JSON.parse(savedTasks)
  }
})

watch(tasks, (newTasks) => {
  localStorage.setItem('my-todo-tasks', JSON.stringify(newTasks))
}, { deep: true })

const totalPages = computed(() => {
  return Math.ceil(tasks.value.length / itemsPerPage) || 1
})

const paginatedTasks = computed(() => {
  const start = (currentPage.value - 1) * itemsPerPage
  const end = start + itemsPerPage
  return tasks.value.slice(start, end)
})

function addTask() {
  if (newTask.value.trim() !== '') {
    tasks.value.unshift({
      id: Date.now(),
      text: newTask.value,
      completed: false
    })
    newTask.value = ''
    currentPage.value = 1
  }
}

function removeTask(taskToRemove) {
  tasks.value = tasks.value.filter(task => task.id !== taskToRemove.id)
  
  if (currentPage.value > totalPages.value) {
    currentPage.value = totalPages.value
  }
}

function completeTask(task) {
  task.completed = true
  setTimeout(() => {
    removeTask(task)
  }, 3000)
}

function nextPage() {
  if (currentPage.value < totalPages.value) {
    currentPage.value++
  }
}

function prevPage() {
  if (currentPage.value > 1) {
    currentPage.value--
  }
}
</script>

<template>
  <div class="page-wrapper">
    <div class="todo-app">
      <h1>Minha To-Do List</h1>

      <div class="input-group">
        <input
          v-model="newTask"
          @keyup.enter="addTask"
          placeholder="O que precisa ser feito?"
          class="task-input"
        >
        <button class="btn-add" @click="addTask">
          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <line x1="12" y1="5" x2="12" y2="19"></line>
            <line x1="5" y1="12" x2="19" y2="12"></line>
          </svg>
        </button>
      </div>

      <transition-group name="list" tag="ul" :class="{ 'has-tasks': tasks.length > 0 }">
        <li v-for="task in paginatedTasks" :key="task.id" :class="{ 'is-completed': task.completed }">
          <input
            type="checkbox"
            v-model="task.completed"
            @change="completeTask(task)"
            :disabled="task.completed"
          >
          <span class="task-text">{{ task.text }}</span>
          <button class="btn-remove" @click="removeTask(task)" :disabled="task.completed">Remover</button>
        </li>
      </transition-group>

      <div class="pagination" v-if="tasks.length > itemsPerPage">
        <button @click="prevPage" :disabled="currentPage === 1">Anterior</button>
        <span>Página {{ currentPage }} de {{ totalPages }}</span>
        <button @click="nextPage" :disabled="currentPage === totalPages">Próxima</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.page-wrapper {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #f0f2f5;
}

.todo-app {
  width: 100%;
  max-width: 450px;
  background: white;
  padding: 30px;
  border-radius: 12px;
  box-shadow: 0 10px 25px rgba(0,0,0,0.1);
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

h1 {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 25px;
  font-size: 1.8rem;
}

.input-group {
  display: flex;
  gap: 12px;
  margin-bottom: 30px;
}

.task-input {
  flex: 1;
  padding: 12px 16px;
  border: 1px solid #d1d5db;
  border-radius: 8px;
  font-size: 1rem;
  color: #334155;
  outline: none;
  background-color: #f8fafc;
  transition: all 0.3s ease;
}

.task-input:focus {
  border-color: #42b883;
  background-color: #ffffff;
  box-shadow: 0 0 0 3px rgba(66, 184, 131, 0.2);
}

.btn-add {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 48px;
  height: 48px;
  padding: 0;
  background-color: #42b883;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.3s, transform 0.1s;
}

.btn-add:hover {
  background-color: #33a06f;
}

.btn-add:active {
  transform: scale(0.95);
}

ul {
  list-style: none;
  padding: 0;
  margin: 0;
  transition: min-height 0.3s ease;
}

ul.has-tasks {
  min-height: 340px;
}

li {
  display: flex;
  align-items: center;
  gap: 15px;
  background: #ffffff;
  padding: 15px;
  margin-bottom: 12px;
  border-radius: 8px;
  border: 1px solid #e2e8f0;
  box-shadow: 0 2px 4px rgba(0,0,0,0.02);
  transition: all 0.3s ease;
}

.task-text {
  flex: 1;
  color: #334155;
  font-size: 1.05rem;
  transition: color 0.3s;
}

.btn-remove {
  padding: 6px 12px;
  background-color: #ef4444;
  color: white;
  border: none;
  border-radius: 6px;
  font-size: 0.85rem;
  cursor: pointer;
  transition: background-color 0.3s;
}

.btn-remove:hover:not(:disabled) {
  background-color: #dc2626;
}

.is-completed {
  opacity: 0.6;
  background-color: #f1f5f9;
}

.is-completed .task-text {
  text-decoration: line-through;
  color: #94a3b8;
}

button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}

.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(-30px);
}

.pagination {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 20px;
  font-size: 0.9rem;
  color: #64748b;
}

.pagination button {
  padding: 8px 15px;
  background-color: #e2e8f0;
  color: #334155;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-weight: 600;
  transition: background-color 0.3s;
}

.pagination button:hover:not(:disabled) {
  background-color: #cbd5e1;
}
</style>