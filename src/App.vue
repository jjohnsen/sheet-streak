<script setup lang="ts">
import { ref, computed } from 'vue'
import StatCounter from './components/StatCounter.vue' // <-- ADD THIS
import Confetti from './components/Confetti.vue';
import LevelBar, { xpForWeek } from './components/LevelBar.vue';

const params = new URLSearchParams(window.location.search);
const weekParam = params.get('week') || '1';
const week = ref<number>(parseInt(weekParam));
const weekDigits = computed( () => 
  week.value.toString().padStart(2, '0').split('')
)
import bg1 from './assets/background-1.jpg';
import bg2 from './assets/background-2.jpg';
import bg3 from './assets/background-3.jpg';
import bg4 from './assets/background-4.jpg';

const backgrounds = [bg1, bg2, bg3, bg4];
console.log('Backgrounds:', backgrounds);
const background = backgrounds[week.value % 4];
const backgroundImage = ref(`url('${background}')`);
</script>

<template>
  <Confetti />
  <div class="game-screen">
    <div class="hud-overlay">
      
      <h1 class="title">{{ week }} WEEK STREAK!</h1>
      
      <div class="streak-display">
        <div v-for="(digit, index) in weekDigits" :key="index" class="digit-card">
          {{ digit }}
        </div>
      </div>

      <h2 class="subtitle">STREAK TOTALS</h2>
    
      <div class="stats-grid">
        <StatCounter :value="Math.round( week * (40 + Math.random()))">HOURS BILLED</StatCounter>
        <StatCounter :value="week">SHEETS DONE</StatCounter>
        <StatCounter :value="xpForWeek(week)">TOTAL XP</StatCounter>
      </div>

      <LevelBar :week="week" />
      <!--
      <button class="zwift-btn">CONTINUE</button>
      -->
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
  background: linear-gradient(rgba(0, 0, 0, 0.3), rgba(0, 0, 0, 0.6));
  background-image: v-bind('backgroundImage');
  /*background-image: url('assets/background-1.jpg');*/
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