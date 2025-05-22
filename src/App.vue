<template>
  <div class="p-4 max-w-md mx-auto bg-white rounded shadow">
    <h1 class="text-2xl font-bold mb-4 text-center text-blue-600">🕒 ポモドーロタイマー</h1>
    <p class="text-center text-gray-700 mb-6">{{ status }} - {{ formattedTime }}</p>
    <div class="flex justify-center space-x-4">
      <button class="bg-blue-500 hover:bg-blue-600 text-white px-4 py-2 rounded" @click="startTimer">スタート</button>
      <button class="bg-yellow-400 hover:bg-yellow-500 text-white px-4 py-2 rounded" @click="pauseTimer">一時停止</button>
      <button class="bg-gray-400 hover:bg-gray-500 text-white px-4 py-2 rounded" @click="resetTimer">リセット</button>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

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

<!-- style scoped は外してOK -->
<style>
/* Tailwindを使うならここは空か競合しないCSSだけに */
</style>

