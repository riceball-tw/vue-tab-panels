<template>
  <div class="tab-view">
    <div class="tab-controls">
      <div class="tab-list" role="tablist">
        <div
          v-for="tab in openTabs"
          :key="tab.id"
          :class="[
            'tab-item',
            { active: activeTab === tab.id }
          ]"
        >
          <button
            :class="['tab-button']"
            @click="setActiveTab(tab.id)"
            :aria-selected="activeTab === tab.id"
            role="tab"
          >
            {{ tab.label }}
          </button>
          <button
            v-if="tab.closable !== false"
            @click="closeTab(tab.id)"
            class="close-button"
            :aria-label="`Close ${tab.label} tab`"
          >
            ×
          </button>
        </div>
      </div>
      

      </div>
    </div>
    
    <div class="tab-content">
      <div
        v-for="tab in openTabs"
        :key="tab.id"
        v-show="activeTab === tab.id"
        class="tab-panel"
        role="tabpanel"
      >
        <component
          :is="tab.component"
          v-bind="tab.props"
          :tabId="tab.id"
          :ref="(el) => setTabRef(tab.id, el)"
          @close="closeTab"
        />
      </div>
    </div>
        <div class="reopen-buttons">
        <button
          v-for="tab in closedTabs"
          :key="tab.id"
          @click="reopenTab(tab.id)"
          class="reopen-button"
        >
          {{ tab.label }}
        </button>
  </div>
  
</template>

<script setup lang="ts">
import { ref, onMounted, watch, computed } from 'vue'

interface Tab {
  id: string
  label: string
  component: any
  props?: Record<string, any>
  closable?: boolean
}

interface Props {
  tabs: Tab[]
  defaultActiveTab?: string
}

const props = withDefaults(defineProps<Props>(), {
  defaultActiveTab: ''
})

const activeTab = ref<string>('')
const tabRefs = ref<Map<string, any>>(new Map())
const closedTabIds = ref<Set<string>>(new Set())

const openTabs = computed(() => 
  props.tabs.filter(tab => !closedTabIds.value.has(tab.id))
)

const closedTabs = computed(() => 
  props.tabs.filter(tab => closedTabIds.value.has(tab.id))
)

const setTabRef = (tabId: string, el: any) => {
  if (el) {
    tabRefs.value.set(tabId, el)
  }
}

const setActiveTab = (tabId: string) => {
  activeTab.value = tabId
}

const closeTab = (tabId: string) => {
  closedTabIds.value.add(tabId)
  
  if (activeTab.value === tabId) {
    const remainingTabs = openTabs.value
    if (remainingTabs.length > 0) {
      activeTab.value = remainingTabs[0].id
    } else {
      activeTab.value = ''
    }
  }
}

const reopenTab = (tabId: string) => {
  closedTabIds.value.delete(tabId)
  activeTab.value = tabId
}

onMounted(() => {
  if (props.defaultActiveTab && props.tabs.some(tab => tab.id === props.defaultActiveTab)) {
    activeTab.value = props.defaultActiveTab
  } else if (props.tabs.length > 0) {
    activeTab.value = props.tabs[0].id
  }
})

watch(() => props.tabs, (newTabs) => {
  if (newTabs.length > 0 && (!activeTab.value || !newTabs.some(tab => tab.id === activeTab.value))) {
    const availableTabs = newTabs.filter(tab => !closedTabIds.value.has(tab.id))
    if (availableTabs.length > 0) {
      activeTab.value = availableTabs[0].id
    }
  }
}, { immediate: true })
</script>

<style scoped>
.tab-view {
  width: 100%;
}

.tab-controls {
  display: flex;
  align-items: center;
  gap: 16px;
  border-bottom: 1px solid #e5e7eb;
  background-color: #f9fafb;
}

.tab-list {
  display: flex;
  flex: 1;
}

.tab-item {
  display: flex;
  align-items: center;
  border-bottom: 2px solid transparent;
  transition: all 0.2s ease;
}

.tab-item.active {
  border-bottom-color: #2563eb;
  background-color: #ffffff;
}

.tab-button {
  padding: 12px 16px;
  border: none;
  background: none;
  cursor: pointer;
  font-size: 14px;
  font-weight: 500;
  color: #6b7280;
  transition: all 0.2s ease;
}

.tab-button:hover {
  color: #374151;
}

.tab-item.active .tab-button {
  color: #2563eb;
}

.close-button {
  padding: 4px 8px;
  margin-left: 4px;
  border: none;
  background: none;
  cursor: pointer;
  font-size: 16px;
  color: #9ca3af;
  border-radius: 4px;
  transition: all 0.2s ease;
}

.close-button:hover {
  color: #ef4444;
  background-color: #fee2e2;
}

.reopen-buttons {
  display: flex;
  gap: 8px;
}

.reopen-button {
  padding: 8px 16px;
  border: 1px solid #d1d5db;
  background-color: #ffffff;
  cursor: pointer;
  font-size: 14px;
  border-radius: 4px;
  transition: all 0.2s ease;
  color: #6b7280;
}

.reopen-button:hover {
  background-color: #f3f4f6;
  color: #374151;
  border-color: #9ca3af;
}

.tab-content {
  padding: 24px;
  background-color: #ffffff;
}

.tab-panel {
  min-height: 200px;
}
</style>