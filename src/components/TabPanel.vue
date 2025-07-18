<template>
  <div class="tab-panel">
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
</script>

<style scoped>
.tab-panel {
  width: 100%;
  height: 100%;
}
</style>