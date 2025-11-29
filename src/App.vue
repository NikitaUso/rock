<script setup>
import { ref } from 'vue'

// Tillstånd för spelet: 'idle', 'counting', 'result'
const gameState = ref('idle')
const count = ref(3)
// Beats sequence shown during countdown
const beats = ['3', '2', '1']
const beatIndex = ref(0)
const result = ref(null)
const resultIcon = ref('')

// Alternativen
const options = [
  { name: 'Sten', icon: '🪨' },
  { name: 'Sax', icon: '✂️' },
  { name: 'Påse', icon: '💰' }
]

const startGame = () => {
  // Återställ variabler
  gameState.value = 'counting'
  // start at first beat
  beatIndex.value = 0
  count.value = 3
  result.value = null
  
  // Starta timer: total tid 2000ms men visa 3 tydliga takter/beats
  const stepMs = Math.round(2000 / 3)
  const timer = setInterval(() => {
    // gå till nästa beat
    beatIndex.value++

    // uppdatera count för kompatibilitet (visas inte längre som nummer)
    count.value--

    // När vi passerat sista beat
    if (beatIndex.value >= beats.length) {
      clearInterval(timer)
      determineWinner()
    }
  }, stepMs)
}

const determineWinner = () => {
  // Slumpa ett tal mellan 0 och 2
  const randomIndex = Math.floor(Math.random() * options.length)
  const selection = options[randomIndex]
  
  // Sätt resultatet
  result.value = selection.name
  resultIcon.value = selection.icon
  gameState.value = 'result'
}
</script>

<template>
  <main class="container">

    <div v-if="gameState === 'idle'" class="game-area">
      <button @click="startGame" class="start-btn">START</button>
    </div>

    <div v-else-if="gameState === 'counting'" class="game-area">
      <div class="countdown">{{ beats[beatIndex] }}</div>

      <div v-if="beats[beatIndex] === 'PÅSE'" class="pase-note">Om du hänger med: Påse slår Sten men förlorar mot Sax.</div>
    </div>

    <div v-else-if="gameState === 'result'" class="game-area">
      <div class="result-display">
        <span class="icon">{{ resultIcon }}</span>
        <h2 class="result-text">{{ result }}</h2>
      </div>
      <button @click="startGame" class="retry-btn">Spela igen</button>
    </div>
  </main>
</template>

<style scoped>
/* Enkel styling för att centrera allt och göra det snyggt */
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  font-family: 'Arial', sans-serif;
  background-color: #ffbbff;
  color: white;
}

.title {
  font-size: 2rem;
  margin-bottom: 1.5rem;
  text-align: center;
  font-weight: 700;
}

.game-area {
  display: flex;
  flex-direction: column;
  align-items: center;
  min-height: 200px;
}

.start-btn, .retry-btn {
  padding: 15px 40px;
  font-size: 1.5rem;
  font-weight: bold;
  cursor: pointer;
  background-color: #ccccff;
  color: #000;
  border: none;
  border-radius: 8px;
  transition: transform 0.1s, background-color 0.2s;
}

.start-btn:hover, .retry-btn:hover {
  background-color: #b3b3ff;
  transform: scale(1.05);
}

.countdown {
  font-size: 6rem;
  font-weight: bold;
  color: #ffffff;
}

.beats-line {
  margin-top: 12px;
  font-weight: 700;
  color: #222;
}

.beats-line .sep{
  margin: 0 8px;
  opacity: 0.6;
}

.beats-line .current{
  text-decoration: underline;
  color: #000;
}

.pase-note{
  margin-top:10px;
  font-size:0.95rem;
  color:#111;
  background: rgba(255,255,255,0.6);
  padding:8px 12px;
  border-radius:6px;
}

.result-display {
  text-align: center;
  margin-bottom: 20px;
}

.result-display .icon {
  font-size: 6rem;
  display: block;
}

.result-display .icon {
  color: #ffffff;
}

.result-display h2, .result-text {
  font-size: 3rem;
  margin: 10px 0;
  color: #ffffff;
}

/* Animation för nedräkningen */
@keyframes pop {
  0% { transform: scale(0.5); opacity: 0; }
  100% { transform: scale(1); opacity: 1; }
}
</style>
