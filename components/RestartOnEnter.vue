<script setup>
import { ref, watch } from 'vue'
import { useSlideContext } from '@slidev/client'

const { isVisible } = useSlideContext()
const render = ref(true)

// Forza il re-mount del componente quando la slide diventa visibile
watch(isVisible, (visible) => {
  if (visible) {
    render.value = false
    setTimeout(() => { render.value = true }, 10)
  }
})
</script>

<template>
  <slot v-if="render" />
</template>