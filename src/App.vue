<template>
  <div class="container">
    <h1>🕒 ポモドーロタイマー</h1>
    <TimerDisplay :status="status" :time="formattedTime" />
    <TimerControls
      @start="startTimer"
      @pause="pauseTimer"
      @reset="resetTimer"
    />
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import TimerDisplay from './components/TimerDisplay.vue'
import TimerControls from './components/TimerControls.vue'

const workDuration = 25 * 60
const breakDuration = 5 * 60

const time = ref(workDuration)
const status = ref('作業中')
const isRunning = ref(false)
let timer = null

const formattedTime = computed(() => {
  const min = String(Math.floor(time.value / 60)).padStart(2, '0')
  const sec = String(time.value % 60).padStart(2, '0')
  return `${min}:${sec}`
})

function startTimer() {
  if (isRunning.value) return
  isRunning.value = true
  timer = setInterval(() => {
    time.value--
    if (time.value <= 0) {
      switchMode()
    }
  }, 1000)
}

function pauseTimer() {
  isRunning.value = false
  clearInterval(timer)
}

function resetTimer() {
  pauseTimer()
  status.value = '作業中'
  time.value = workDuration
}

function switchMode() {
  pauseTimer()
  if (status.value === '作業中') {
    status.value = '休憩中'
    time.value = breakDuration
  } else {
    status.value = '作業中'
    time.value = workDuration
  }
  startTimer()
}
</script>

<style scoped>
.container {
  text-align: center;
  padding: 2rem;
  font-family: sans-serif;
}
button {
  margin: 0.5rem;
  padding: 0.5rem 1rem;
}
</style>
