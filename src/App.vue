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
const backgroundImage = computed(() =>
  `url('${backgrounds[week.value % 4]}')`
)
const showInfo = ref(false)

const titles = [
  "🔥 Godt jobba!",
  "🤩 Takk for timene!",
  "💪 Bra kontinuitet! ",
  "🚀 Takk for innsendingen!",
  "⭐ Flott levert!",
  "🏆 Stabil innsats",
  "🙌 Herlig innsats",
  "🎯 Jevnt og solid",
  "📘 Takk for at du får dette gjort",
  "👌 Timene er på plass - bra jobba",
  "⚡ Stabil rytme - godt ført!"
];
const title = computed(() => 
  titles[(week.value) % titles.length]
);
</script>

<template>
  <audio id="bg-audio" src="motivation-174649.mp3" autoplay loop></audio>
  <Confetti />
  
  <div class="game-screen">
    
    <div class="hud-overlay">
            <div v-if="showInfo" class="info-overlay" @click.self="showInfo = false">
            <div class="info-box">
              <h3>Hva er dette⁉️</h3>
              <p>
                Dette er en enkel og frivillig streak-visning. Den er laget for å gi litt
                anerkjennelse for jevn timeregistrering.<br>
                Streaken brukes ikke som måling eller krav.<br>
                Pauser kan skje av mange grunner og er helt uproblematiske.
                Dette er kun en liten takk for at du registrerer timer til avtalt tid🤩.
              </p>

              <button @click="showInfo = false" class="zwift-btn">Lukk</button>
            </div>
          </div>
      <div class="info-button" @click="showInfo = true">⁉️</div>

      
      <h1 class="title">{{ title }}</h1>
      
      <div class="streak-display">
        <div v-for="(digit, index) in weekDigits" :key="index" class="digit-card">
          {{ digit }}
        </div>
      </div>

      <h2 class="subtitle">WEEK STREAK</h2>
    
      <div class="stats-grid">
        <StatCounter :value="Math.round( week * (40 + Math.random()))">Timer ført</StatCounter>
        <StatCounter :value="week">Timelister</StatCounter>
        <StatCounter :value="xpForWeek(week)">TOTAL XP</StatCounter>
      </div>

      <LevelBar :week="week" />
      
      <!--<button class="zwift-btn">CONTINUE</button>-->
      
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

.info-button {
  position: absolute;
  top: 20px;
  right: 20px;
  width: 32px;
  height: 32px;
  border-radius: 50%;
  background: rgba(255,255,255,0.9);
  color: #333;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 900;
  cursor: pointer;
  user-select: none;
  box-shadow: 0 2px 6px rgba(0,0,0,0.4);
}

.info-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.6);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1rem;
  z-index: 1000;
}

.info-box {
  background: white;
  color: #333;
  padding: 2rem;
  max-width: 400px;
  border-radius: 12px;
  box-shadow: 0 4px 20px rgba(0,0,0,0.4);
  text-transform: none;
  box-shadow: none;
  text-shadow: none;;
}

.info-box h3 {
  margin-top: 0;
  margin-bottom: 1rem;
  font-size: 1.2rem;
}

.info-box button {
  margin-top: 1.5rem;
  padding: 0.6rem 1.2rem;
  border: none;
  background: #fc6719;
  color: white;
  cursor: pointer;
  border-radius: 6px;
}

</style>