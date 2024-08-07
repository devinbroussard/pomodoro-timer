<script setup>
import { ref, computed } from 'vue'

const maxTimer = 1500
const timer = ref(maxTimer)

let timerInterval = null

function restartTimer() {
  timer.value = maxTimer
  if (timerInterval) timerInterval()

  timerInterval = setInterval(() => {
    if (timer.value === 0) timerInterval()

    timer.value--
  }, 1000)
}

const displayTime = computed(() => {
  const minutes = Math.floor(timer.value / 60)
  const seconds = timer.value - minutes * 60

  return `${minutes || '00'}:${seconds || '00'}`
})

restartTimer()
</script>

<template>
  <div>{{ displayTime }}</div>

  <button @click="restartTimer()">Restart time!</button>
</template>

<style scoped></style>
