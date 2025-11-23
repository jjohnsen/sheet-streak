<script lang="ts">
  const xpPerWeek = 250;

  export const xpForWeek = function(week: number): number {
    return week * xpPerWeek;
  }

  export const level = function(xp: number): number {
    const lvl = 0.15 * Math.sqrt(xp);
    return Math.floor(lvl);
  }
  export const xpForLevel = function(level: number): number {
    return Math.round( (level / 0.15) ** 2 );
  }
</script>
<script setup lang="ts">
  import { computed, onMounted, ref } from 'vue';

  const props = defineProps<{
    week: number
    duration?: number
  }>();

  const xp = computed(() => xpForWeek(props.week));
  const currentLevel = computed(() => level(xp.value));
  const progressPercentage = ref(0);
  const xpToNextLevel = xpForLevel(currentLevel.value + 1) - xp.value;

  onMounted(() => {
    setTimeout(() => {
      progressPercentage.value = (xp.value / xpForLevel(currentLevel.value + 1)) * 100; // Trigger computed
    }, 500); // Delay before starting animation
  });
</script>

<template>
  <div class="level-area">
    <div class="level-info">
        <span class="level-label">CURRENT LEVEL <strong>{{ currentLevel }}</strong></span>
        <div class="xp-badge">📅 SHEET STREAK: +{{ xpPerWeek }} XP</div>
        <span class="level-label">NEXT LEVEL <strong>{{ currentLevel+1 }}</strong></span>
    </div>

    <div class="progress-track">
        <div class="progress-fill" :style="{ width: progressPercentage + '%' }"></div>
    </div>

    <div class="xp-remaining">
        {{ xpToNextLevel }} XP UNTIL NEXT LEVEL
    </div>
  </div>
</template>

<style scoped>
  .level-area {
    margin-bottom: 3rem;
    text-align: left;
  }

  .level-info {
    display: flex;
    justify-content: space-between;
    align-items: flex-end;
    margin-bottom: 5px;
    font-size: 0.9rem;
    font-weight: 700;
  }

  .xp-badge {
    font-size: 1rem;
    font-weight: 800;
  }

  .progress-track {
    background: rgba(255, 255, 255, 0.3);
    height: 24px;
    border-radius: 12px;
    overflow: hidden;
    border: 2px solid rgba(255, 255, 255, 0.5);
  }

  .progress-fill {
    background-color: #0099ff; /* Zwift Blue */
    height: 100%;
    border-radius: 10px;
    transition: width 5s ease-out;
  }

  .xp-remaining {
    text-align: right;
    font-size: 0.7rem;
    margin-top: 5px;
    opacity: 0.8;
  }
</style>