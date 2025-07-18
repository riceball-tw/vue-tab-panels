<template>
  <div class="demo-panel">
    <button @click="handleClose" class="panel-close-btn">×</button>
    <h2>Panel 1 - Counter Demo</h2>
    <p>This panel demonstrates state preservation. The counter value persists when you switch tabs.</p>
    
    <div class="counter-section">
      <h3>Counter: {{ count }}</h3>
      <div class="button-group">
        <button @click="increment" class="btn btn-primary">+</button>
        <button @click="decrement" class="btn btn-secondary">-</button>
        <button @click="reset" class="btn btn-danger">Reset</button>
      </div>
    </div>
    
    <div class="input-section">
      <h3>Text Input (also preserved):</h3>
      <input 
        v-model="textInput" 
        type="text" 
        placeholder="Type something..."
        class="text-input"
      />
      <p v-if="textInput">You typed: {{ textInput }}</p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

interface Props {
  tabId?: string
}

const props = withDefaults(defineProps<Props>(), {
  tabId: ''
})

const count = ref(0)
const textInput = ref('')

const emit = defineEmits<{
  close: [tabId: string]
}>()

const increment = () => {
  count.value++
}

const decrement = () => {
  count.value--
}

const reset = () => {
  count.value = 0
}

const handleClose = () => {
  emit('close', props.tabId)
}
</script>

<style scoped>
.demo-panel {
  padding: 20px;
  position: relative;
}

.panel-close-btn {
  position: absolute;
  top: 10px;
  right: 10px;
  padding: 4px 8px;
  border: none;
  background: rgba(239, 68, 68, 0.1);
  color: #dc2626;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
  font-weight: bold;
  transition: all 0.2s ease;
}

.panel-close-btn:hover {
  background: rgba(239, 68, 68, 0.2);
  color: #b91c1c;
}

.counter-section {
  margin: 20px 0;
  padding: 20px;
  border: 1px solid #e5e7eb;
  border-radius: 8px;
}

.button-group {
  display: flex;
  gap: 10px;
  margin-top: 10px;
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

.btn-secondary {
  background-color: #6b7280;
  color: white;
}

.btn-danger {
  background-color: #dc2626;
  color: white;
}

.btn:hover {
  opacity: 0.9;
}

.input-section {
  margin: 20px 0;
  padding: 20px;
  border: 1px solid #e5e7eb;
  border-radius: 8px;
}

.text-input {
  width: 100%;
  padding: 8px 12px;
  border: 1px solid #d1d5db;
  border-radius: 4px;
  font-size: 14px;
  margin-top: 8px;
}

.text-input:focus {
  outline: none;
  border-color: #2563eb;
  box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.1);
}
</style>