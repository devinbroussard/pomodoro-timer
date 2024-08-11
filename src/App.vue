<script setup>
import { ref, computed } from 'vue'

const maxPomodoroTimer = 1500
const maxShortBreakTimer = 300
const maxLongBreakTimer = 600

let currentMaxTimer = maxPomodoroTimer

const timer = ref(maxPomodoroTimer)
const isTimerPaused = ref(false)

let timerInterval = null

function restartTimer() {
  timer.value = currentMaxTimer
  if (timerInterval) timerInterval()

  timerInterval = setInterval(() => {
    if (isTimerPaused.value) return

    if (timer.value === 0) {
      console.log('test')
      clearInterval(timerInterval)
      timerInterval = null
      timer.value = currentMaxTimer

      return
    }

    timer.value--
  }, 1000)
}

const displayTime = computed(() => {
  const minutes = Math.floor(timer.value / 60)
  const seconds = timer.value - minutes * 60

  return `${minutes || '00'}:${seconds || '00'}`
})

function changeTimer(maxTimer) {
  currentMaxTimer = maxTimer
  timer.value = currentMaxTimer

  clearInterval(timerInterval)
  timerInterval = null
}
</script>

<template>
  <div>
    <div>
      <button @click="changeTimer(maxPomodoroTimer)" type="button">Pomodoro</button>
      <button @click="changeTimer(maxShortBreakTimer)" type="button">Short Break</button>
      <button @click="changeTimer(maxLongBreakTimer)" type="button">Long Break</button>
    </div>
    <div>{{ displayTime }}</div>
    <button type="button" @click="restartTimer()">{{ timerInterval ? 'Reset' : 'Start' }}</button>
    <button type="button" v-if="timerInterval" @click="isTimerPaused = !isTimerPaused">
      {{ isTimerPaused ? 'Start' : 'Pause' }}
    </button>
  </div>
</template>

<style scoped></style>
