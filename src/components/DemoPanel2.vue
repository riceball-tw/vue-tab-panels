<template>
  <div class="demo-panel">
    <h2>Panel 2 - Todo List Demo</h2>
    <p>This panel shows a todo list. Add items, mark them complete, and see how the state is preserved when switching tabs.</p>
    
    <div class="todo-section">
      <div class="add-todo">
        <input 
          v-model="newTodo" 
          @keyup.enter="addTodo"
          type="text" 
          placeholder="Add a new todo..."
          class="todo-input"
        />
        <button @click="addTodo" class="btn btn-primary">Add</button>
      </div>
      
      <div class="todo-list">
        <div 
          v-for="todo in todos" 
          :key="todo.id"
          class="todo-item"
          :class="{ completed: todo.completed }"
        >
          <input 
            type="checkbox" 
            v-model="todo.completed"
            class="todo-checkbox"
          />
          <span class="todo-text">{{ todo.text }}</span>
          <button @click="removeTodo(todo.id)" class="btn btn-danger btn-sm">Delete</button>
        </div>
        
        <p v-if="todos.length === 0" class="empty-state">No todos yet. Add one above!</p>
      </div>
      
      <div class="todo-stats">
        <p>Total: {{ todos.length }} | Completed: {{ completedCount }} | Remaining: {{ remainingCount }}</p>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'

interface Todo {
  id: number
  text: string
  completed: boolean
}

const todos = ref<Todo[]>([])
const newTodo = ref('')
let nextId = 1

const addTodo = () => {
  if (newTodo.value.trim()) {
    todos.value.push({
      id: nextId++,
      text: newTodo.value.trim(),
      completed: false
    })
    newTodo.value = ''
  }
}

const removeTodo = (id: number) => {
  const index = todos.value.findIndex(todo => todo.id === id)
  if (index > -1) {
    todos.value.splice(index, 1)
  }
}

const completedCount = computed(() => 
  todos.value.filter(todo => todo.completed).length
)

const remainingCount = computed(() => 
  todos.value.filter(todo => !todo.completed).length
)
</script>

<style scoped>
.demo-panel {
  padding: 20px;
}

.todo-section {
  margin: 20px 0;
  padding: 20px;
  border: 1px solid #e5e7eb;
  border-radius: 8px;
}

.add-todo {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.todo-input {
  flex: 1;
  padding: 8px 12px;
  border: 1px solid #d1d5db;
  border-radius: 4px;
  font-size: 14px;
}

.todo-input:focus {
  outline: none;
  border-color: #2563eb;
  box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.1);
}

.btn {
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-weight: 500;
}

.btn-primary {
  background-color: #2563eb;
  color: white;
}

.btn-danger {
  background-color: #dc2626;
  color: white;
}

.btn-sm {
  padding: 4px 8px;
  font-size: 12px;
}

.btn:hover {
  opacity: 0.9;
}

.todo-list {
  min-height: 100px;
}

.todo-item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px;
  border: 1px solid #e5e7eb;
  border-radius: 4px;
  margin-bottom: 8px;
}

.todo-item.completed {
  opacity: 0.6;
  background-color: #f9fafb;
}

.todo-checkbox {
  width: 16px;
  height: 16px;
}

.todo-text {
  flex: 1;
  font-size: 14px;
}

.todo-item.completed .todo-text {
  text-decoration: line-through;
}

.empty-state {
  text-align: center;
  color: #6b7280;
  font-style: italic;
  padding: 40px;
}

.todo-stats {
  margin-top: 20px;
  padding-top: 20px;
  border-top: 1px solid #e5e7eb;
  font-size: 14px;
  color: #6b7280;
}
</style>