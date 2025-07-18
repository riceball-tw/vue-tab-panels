<template>
  <div class="tab-panel">
    <button 
      @click="handleClose" 
      class="panel-close-button"
      :aria-label="`Close ${label} panel`"
    >
      ×
    </button>
    <slot />
  </div>
</template>

<script setup lang="ts">
interface Props {
  id: string
  label: string
  disabled?: boolean
}

const props = withDefaults(defineProps<Props>(), {
  disabled: false
})

const emit = defineEmits<{
  close: [id: string]
}>()

const handleClose = () => {
  emit('close', props.id)
}
</script>

<style scoped>
.tab-panel {
  width: 100%;
  height: 100%;
  position: relative;
}

.panel-close-button {
  position: absolute;
  top: 8px;
  right: 8px;
  padding: 4px 8px;
  border: none;
  background: rgba(0, 0, 0, 0.1);
  cursor: pointer;
  font-size: 16px;
  color: #6b7280;
  border-radius: 4px;
  transition: all 0.2s ease;
  z-index: 10;
}

.panel-close-button:hover {
  color: #ef4444;
  background-color: rgba(239, 68, 68, 0.1);
}
</style>