<script setup lang="ts">
import { ref, computed, onMounted } from 'vue'


const inputStreak = ref<number>(0)

// 2. LOGIC ENGINE (Everything derives from inputStreak)
const totalHours = computed(() => inputStreak.value * 40) // 40h work week
const totalSheets = computed(() => inputStreak.value)
const totalXP = computed(() => inputStreak.value * 100) // 100XP per sheet

// Leveling logic: Level up every 500XP (5 weeks)
const currentLevel = computed(() => Math.floor(totalXP.value / 500) + 1)
const nextLevel = computed(() => currentLevel.value + 1)

// Progress to next level
const xpForCurrentLevelStart = computed(() => (currentLevel.value - 1) * 500)
const xpToNextLevelTotal = 500
const currentLevelXP = computed(() => totalXP.value - xpForCurrentLevelStart.value)
const progressPercentage = computed(() => (currentLevelXP.value / xpToNextLevelTotal) * 100)

// Formatting the big digits (e.g. "2" -> "0" "2")
const streakDigits = computed(() => {
  const s = inputStreak.value.toString().padStart(2, '0')
  return s.split('') // ['0', '2']
})

// 3. INITIALIZATION
onMounted(() => {
  // Grab ?streak=X from the browser URL
  const params = new URLSearchParams(window.location.search)
  const streakParam = params.get('week')
  
  if (streakParam) {
    inputStreak.value = parseInt(streakParam, 10)
  } else {
    // Default for demo purposes if no URL param exists
    inputStreak.value = 2 
  }
})
</script>

<template>
  <div class="game-screen">
    <div class="hud-overlay">
      
      <h1 class="title">{{ inputStreak }} WEEK STREAK!</h1>

      <div class="streak-display">
        <div v-for="(digit, index) in streakDigits" :key="index" class="digit-card">
          {{ digit }}
        </div>
      </div>

      <h2 class="subtitle">STREAK TOTALS</h2>

      <div class="stats-grid">
        <div class="stat-item">
          <span class="stat-value">{{ totalHours }}</span>
          <span class="stat-label">HOURS BILLED</span>
        </div>
        <div class="stat-item">
          <span class="stat-value">{{ totalSheets }}</span>
          <span class="stat-label">SHEETS DONE</span>
        </div>
        <div class="stat-item">
          <span class="stat-value">{{ totalXP }}</span>
          <span class="stat-label">TOTAL XP</span>
        </div>
      </div>

      <div class="level-area">
        <div class="level-info">
          <span class="level-label">CURRENT LEVEL <strong>{{ currentLevel }}</strong></span>
          <div class="xp-badge">📅 SHEET STREAK: +100 XP</div>
          <span class="level-label">NEXT LEVEL <strong>{{ nextLevel }}</strong></span>
        </div>

        <div class="progress-track">
          <div class="progress-fill" :style="{ width: progressPercentage + '%' }"></div>
        </div>
        
        <div class="xp-remaining">
          {{ xpToNextLevelTotal - currentLevelXP }} XP UNTIL NEXT LEVEL
        </div>
      </div>

      <button class="zwift-btn">CONTINUE</button>

    </div>
  </div>
</template>

<style scoped>
/* ZWIFT THEME VARS */
:root {
  --z-orange: #fc6719;
  --z-blue: #0099ff;
  --z-dark: #222;
  --z-white: #fff;
}

.game-screen {
  width: 100vw;
  height: 100vh;
  background: linear-gradient(rgba(0, 0, 0, 0.3), rgba(0, 0, 0, 0.6)), url('assets/background-1.jpg');
  background-size: cover;
  background-position: center;
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: 'Avenir', 'Helvetica Neue', sans-serif;
  color: white;
  text-transform: uppercase;
}

.hud-overlay {
  text-align: center;
  max-width: 800px;
  width: 100%;
  padding: 2rem;
  /* Slight text shadow for readability against background */
  text-shadow: 0 2px 4px rgba(0,0,0,0.8);
}

.title {
  font-size: 2.5rem;
  font-weight: 900;
  margin-bottom: 1rem;
  letter-spacing: 1px;
}

/* THE BIG NUMBERS */
.streak-display {
  display: flex;
  justify-content: center;
  gap: 1rem;
  margin-bottom: 0.5rem;
}

.digit-card {
  background: white;
  color: #333;
  font-size: 5rem;
  font-weight: 800;
  width: 100px;
  height: 120px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 8px;
  box-shadow: 0 10px 20px rgba(0,0,0,0.5);
  text-shadow: none;
}

.subtitle {
  font-size: 1.2rem;
  font-weight: 800;
  margin-bottom: 2rem;
  opacity: 0.9;
}

/* STATS GRID */
.stats-grid {
  display: flex;
  justify-content: space-around;
  margin-bottom: 3rem;
}

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

/* LEVEL PROGRESS BAR */
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
  transition: width 1s ease-out;
}

.xp-remaining {
  text-align: right;
  font-size: 0.7rem;
  margin-top: 5px;
  opacity: 0.8;
}

/* BUTTON */
.zwift-btn {
  background-color: #fc6719; /* Zwift Orange */
  color: white;
  border: none;
  padding: 15px 60px;
  font-size: 1.2rem;
  font-weight: 800;
  text-transform: uppercase;
  border-radius: 4px;
  cursor: pointer;
  box-shadow: 0 4px 0 #b04209; /* Darker orange for 3D effect */
  transition: transform 0.1s;
}

.zwift-btn:active {
  transform: translateY(4px);
  box-shadow: none;
}
</style>