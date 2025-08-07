<script setup>
import { ref, computed } from 'vue'
import { Analytics } from '@vercel/analytics/nuxt'
// Timer state
const timeInMinutes = ref(25)
const isRunning = ref(false)
const seconds = ref(0)
let timerInterval

// Import background images
import bgEvening from '../public/assets/Cyberpunk2077-Evening.jpg'
import bgWatson from '../public/assets//Cyberpunk2077-Watson.jpg'
import bgWestbrook from '../public/assets/Cyberpunk2077-Westbrook.jpg'
import bgFlat from '../public/assets/flat-DS1.jpg'
import bgWaterfall from '../public/assets/Waterfall-DS1.jpg'

// Timer state
// ...existing code...

// Background customization
const backgrounds = [
  `url(${bgEvening})`,
  `url(${bgWatson})`,
  `url(${bgWestbrook})`,
  `url(${bgFlat})`,
  `url(${bgWaterfall})`,
]

const currentBgIndex = ref(0)
const currentBackground = computed(() => ({
  backgroundImage: backgrounds[currentBgIndex.value],
  backgroundSize: 'cover',
  backgroundPosition: 'center',
}))

// Timer functions
const startTimer = () => {
  if (!isRunning.value) {
    isRunning.value = true
    timerInterval = setInterval(() => {
      if (seconds.value === 0) {
        if (timeInMinutes.value === 0) {
          stopTimer()
          return
        }
        timeInMinutes.value--
        seconds.value = 59
      } else {
        seconds.value--
      }
    }, 1000)
  }
}

const stopTimer = () => {
  isRunning.value = false
  clearInterval(timerInterval)
}

const resetTimer = () => {
  stopTimer()
  timeInMinutes.value = 25
  seconds.value = 0
}

const changeBackground = () => {
  currentBgIndex.value = (currentBgIndex.value + 1) % backgrounds.length
}

const formattedTime = computed(() => {
  const mins = timeInMinutes.value.toString().padStart(2, '0')
  const secs = seconds.value.toString().padStart(2, '0')
  return `${mins}:${secs}`
})
</script>

<template>
  <analytics />
  <main :style="currentBackground">
    <header class="relative backdrop-blur-lg">
      <h1 class="text-4xl">Pomodoro Timer</h1>
      <h3 class="text-3xl">Focus on your tasks with timed intervals</h3>
      <nav>
        <ul>
          <li>
            <button
              @click="changeBackground"
              class="hover:shadow-2xl hover:scale-110 duration-300 shadow-black"
            >
              Change background
            </button>
          </li>
        </ul>
      </nav>
    </header>
    <section class="timer-section">
      <div class="timer-display">
        <span class="time text-white">{{ formattedTime }}</span>
        <div class="timer-controls">
          <button
            @click="startTimer"
            :disabled="isRunning"
            class="hover:shadow-2xl duration-200 shadow-black"
          >
            Start
          </button>
          <button
            @click="stopTimer"
            :disabled="!isRunning"
            class="hover:shadow-2xl duration-200 shadow-black"
          >
            Stop
          </button>
          <button @click="resetTimer" class="hover:shadow-2xl duration-200 shadow-black">
            Reset
          </button>
        </div>
      </div>
    </section>
  </main>
</template>

<style scoped>
main {
  min-height: 100dvh;
  transition: background-image 0.5s ease;
}
header {
  text-align: center;
  color: white;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
  border-color: white;
}

nav ul {
  display: flex;
  justify-content: center;
  gap: 2rem;
  list-style: none;
  padding: 0;
}

.timer-section {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 60vh;
}

.timer-display {
  background: transparent;
  border-color: white;
  border-width: 1px;
  border-radius: 15px;
  padding: 2rem;
  border-radius: 1rem;
  text-align: center;
  backdrop-filter: blur(5px);
}

.time {
  font-size: 4rem;
  font-weight: bold;
  display: block;
  margin-bottom: 1rem;
}

.timer-controls {
  display: flex;
  gap: 1rem;
  justify-content: center;
}

button {
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 0.5rem;
  background: #4a4a4a00;
  color: white;
  cursor: pointer;
  transition: background 0.3s;
}
</style>
