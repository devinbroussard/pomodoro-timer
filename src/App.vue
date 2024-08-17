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
  if (timerInterval) {
    clearInterval(timerInterval)
    timerInterval = null
    return
  }

  handleInterval()
  timerInterval = setInterval(() => {
    handleInterval()
  }, 1000)
}

function handleInterval() {
  if (isTimerPaused.value) return

  if (timer.value === 0) {
    console.log('test')
    clearInterval(timerInterval)
    timerInterval = null
    timer.value = currentMaxTimer

    return
  }

  timer.value--
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
  <div class="background"></div>
  <div style="display: flex; align-items: center; flex-direction: column; justify-content: center">
    <div class="card">
      <div style="display: flex; gap: 0.5rem; justify-content: space-between; width: 100%">
        <button class="btn" @click="changeTimer(maxPomodoroTimer)" type="button">Pomodoro</button>
        <button class="btn" @click="changeTimer(maxShortBreakTimer)" type="button">Short Break</button>
        <button class="btn" @click="changeTimer(maxLongBreakTimer)" type="button">Long Break</button>
      </div>
      <div style="font-size: 120px; font-weight: bold; line-height: 1">{{ displayTime }}</div>
      <div style="gap: 1rem; display: flex">
        <button
          class="icon-btn"
          type="button"
          v-if="timerInterval"
          @click="isTimerPaused = !isTimerPaused"
        >
          <svg
            v-if="isTimerPaused"
            xmlns="http://www.w3.org/2000/svg"
            width="50"
            height="50"
            fill="currentColor"
            class="bi bi-play-fill"
            viewBox="0 0 16 16"
          >
            <path
              d="m11.596 8.697-6.363 3.692c-.54.313-1.233-.066-1.233-.697V4.308c0-.63.692-1.01 1.233-.696l6.363 3.692a.802.802 0 0 1 0 1.393"
            />
          </svg>
          <svg
            v-if="!isTimerPaused"
            xmlns="http://www.w3.org/2000/svg"
            width="50"
            height="50"
            fill="currentColor"
            class="bi bi-pause"
            viewBox="0 0 16 16"
          >
            <path
              d="M6 3.5a.5.5 0 0 1 .5.5v8a.5.5 0 0 1-1 0V4a.5.5 0 0 1 .5-.5m4 0a.5.5 0 0 1 .5.5v8a.5.5 0 0 1-1 0V4a.5.5 0 0 1 .5-.5"
            />
          </svg>
        </button>
        <button class="icon-btn" type="button" @click="restartTimer()">
          <svg
            v-if="!timerInterval"
            xmlns="http://www.w3.org/2000/svg"
            width="50"
            height="50"
            fill="currentColor"
            class="bi bi-play-fill"
            viewBox="0 0 16 16"
          >
            <path
              d="m11.596 8.697-6.363 3.692c-.54.313-1.233-.066-1.233-.697V4.308c0-.63.692-1.01 1.233-.696l6.363 3.692a.802.802 0 0 1 0 1.393"
            />
          </svg>
          <svg
            v-if="timerInterval"
            xmlns="http://www.w3.org/2000/svg"
            width="40"
            height="40"
            fill="currentColor"
            class="bi bi-arrow-clockwise"
            viewBox="0 0 16 16"
          >
            <path
              fill-rule="evenodd"
              d="M8 3a5 5 0 1 0 4.546 2.914.5.5 0 0 1 .908-.417A6 6 0 1 1 8 2z"
            />
            <path
              d="M8 4.466V.534a.25.25 0 0 1 .41-.192l2.36 1.966c.12.1.12.284 0 .384L8.41 4.658A.25.25 0 0 1 8 4.466"
            />
          </svg>
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.background {
  width: 100vw;
  height: 100vh;
  background-image: url('https://aimadeimage.net/wp-content/uploads/2024/05/dark-academia-wallpaper-laptop-4.jpg');
  filter: blur(1.5px) brightness(70%);
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center;
}

.card {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  padding: 0.75rem 1.5rem;
  background: rgba(255, 255, 255, 0.074);
  border-radius: 10px;
  color: rgb(255, 255, 255);
  z-index: 10000;

  gap: 2rem;

  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 2;
}

.icon-btn {
  background-color: unset;
  border: none;
  box-sizing: 0;
  color: white;
  cursor: pointer;
}

.btn {
  background-color: unset;
  border: white 1px solid;
  color: white;
  border-radius: 40px;
  padding: 0.5rem 1rem;
  font-size: 1.15rem;
  cursor: pointer;
}

.btn:hover {
  color: black;
  background-color: white;
}
</style>
