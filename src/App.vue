<template>
  <div class="app">
    <h1>Vue Tab View Demo</h1>
    <p>This demo shows how tab content and state are preserved when switching between tabs. You can close tabs using the × button and reopen them using the "+ Add Tab" button.</p>
    
    <TabView :tabs="tabs" defaultActiveTab="counter" />
  </div>
</template>

<script setup lang="ts">
import TabView from './components/TabView.vue'
import DemoPanel1 from './components/DemoPanel1.vue'
import DemoPanel2 from './components/DemoPanel2.vue'
import DemoPanel3 from './components/DemoPanel3.vue'

const tabs = [
  {
    id: 'counter',
    label: 'Counter',
    component: DemoPanel1,
    closable: true
  },
  {
    id: 'todos',
    label: 'Todo List',
    component: DemoPanel2,
    closable: true
  },
  {
    id: 'form',
    label: 'Form',
    component: DemoPanel3,
    closable: true
  }
]
</script>

<style>
.app {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
}

h1 {
  color: #1f2937;
  margin-bottom: 8px;
}

p {
  color: #6b7280;
  margin-bottom: 32px;
}
</style>
