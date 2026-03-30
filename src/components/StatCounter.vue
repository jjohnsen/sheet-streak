<script setup lang="ts">
import { onMounted, ref, watch } from 'vue'

const props = defineProps<{
  value: number
  duration?: number
}>()

const displayValue = ref(0)

function animateNumber(target: number, duration: number) {
  const start = displayValue.value
  const diff = target - start
  const startTime = performance.now()

  function tick(now: number) {
    const t = Math.min((now - startTime) / duration, 1)
    displayValue.value = Math.floor(start + diff * t)
    if (t < 1) requestAnimationFrame(tick)
  }

  requestAnimationFrame(tick)
}

onMounted(() => {
  animateNumber(props.value, props.duration ?? 3000)
})

watch(() => props.value, (newVal) => {
  animateNumber(newVal, props.duration ?? 3000)
})
</script>

<template>
  <div class="stat-item">
    <span class="stat-value">{{ displayValue }}</span>
    <span class="stat-label"><slot></slot></span>
  </div>
</template>

<style scoped>
.stat-item {
  display: flex;
  flex-direction: column;
}

.stat-value {
  font-size: 1.8rem;
  font-weight: 800;
}

.stat-label {
  font-size: 0.7rem;
  opacity: 0.8;
  letter-spacing: 1px;
}
</style>