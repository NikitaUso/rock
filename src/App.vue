<script setup>
import { ref } from 'vue'

// Tillstånd för spelet: 'idle', 'counting', 'result'
const gameState = ref('idle')
const count = ref(3)
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
  count.value = 3
  result.value = null
  
  // Starta timer
  const timer = setInterval(() => {
    count.value--
    
    // När timern når 0
    if (count.value === 0) {
      clearInterval(timer)
      determineWinner()
    }
  }, 1000)
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
    <h1 class="title">Kampen om KrökanKronan presented by Isak Wallenius</h1>

    <div v-if="gameState === 'idle'" class="game-area">
      <button @click="startGame" class="start-btn">START</button>
    </div>

    <div v-else-if="gameState === 'counting'" class="game-area">
      <div class="countdown">{{ count }}</div>
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
  font-size: 8rem;
  font-weight: bold;
  color: #ffffff;
  animation: pop 0.5s ease-in-out;
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
