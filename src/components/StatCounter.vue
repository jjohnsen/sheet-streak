<script setup lang="ts">
  import { onMounted, ref } from 'vue';

  const props = defineProps<{
    value: number
    duration?: number
  }>();

  const targetValue = ref(0);
  const duration = ref(props.duration || 2000);

  onMounted(() => {
    // Animation logic can be added here if needed
    targetValue.value = props.value;
  });
</script>

<template>
  <div class="stat-item">
    <span class="stat-value"></span>
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

  @property --num {
    syntax: "<integer>";
    initial-value: 0;
    inherits: false;
  }

  .stat-value {
    transition: --num 3s;
    counter-set: num var(--num);  
    --num: v-bind(targetValue);
  }

  .stat-value::after {
    content: counter(num);
  }
</style>