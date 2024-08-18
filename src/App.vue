<script setup>
import { ref, computed } from 'vue'

const maxPomodoroTimer = 1500
const maxShortBreakTimer = 300
const maxLongBreakTimer = 600

let currentMaxTimer = maxPomodoroTimer

const timer = ref(maxPomodoroTimer)
const isTimerPaused = ref(false)
const isViewingAbout = ref(false)

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
    alert('Timer finished!')
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
  <div class="nav-icons">
    <button class="icon-btn" @click="isViewingAbout = false">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        width="40"
        height="40"
        fill="currentColor"
        class="bi bi-house-fill"
        viewBox="0 0 16 16"
      >
        <path
          d="M8.707 1.5a1 1 0 0 0-1.414 0L.646 8.146a.5.5 0 0 0 .708.708L8 2.207l6.646 6.647a.5.5 0 0 0 .708-.708L13 5.793V2.5a.5.5 0 0 0-.5-.5h-1a.5.5 0 0 0-.5.5v1.293z"
        />
        <path d="m8 3.293 6 6V13.5a1.5 1.5 0 0 1-1.5 1.5h-9A1.5 1.5 0 0 1 2 13.5V9.293z" />
      </svg>
    </button>
    <button class="icon-btn" @click="isViewingAbout = true">
      <svg
        style="cursor: pointer"
        xmlns="http://www.w3.org/2000/svg"
        width="40"
        height="40"
        fill="currentColor"
        class="bi bi-person-circle"
        viewBox="0 0 16 16"
      >
        <path d="M11 6a3 3 0 1 1-6 0 3 3 0 0 1 6 0" />
        <path
          fill-rule="evenodd"
          d="M0 8a8 8 0 1 1 16 0A8 8 0 0 1 0 8m8-7a7 7 0 0 0-5.468 11.37C3.242 11.226 4.805 10 8 10s4.757 1.225 5.468 2.37A7 7 0 0 0 8 1"
        />
      </svg>
    </button>
  </div>
  <div style="display: flex; align-items: center; flex-direction: column; justify-content: center">
    <div v-if="!isViewingAbout" class="card">
      <div style="display: flex; gap: 0.5rem; justify-content: space-between; width: 100%">
        <button class="btn" @click="changeTimer(maxPomodoroTimer)" type="button">Pomodoro</button>
        <button class="btn" @click="changeTimer(maxShortBreakTimer)" type="button">
          Short Break
        </button>
        <button class="btn" @click="changeTimer(maxLongBreakTimer)" type="button">
          Long Break
        </button>
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
    <div v-if="isViewingAbout" class="card">
      <img
        class="about-img"
        src="https://lh3.googleusercontent.com/a/ACg8ocJz4Beb1Ghy58mco0L3_B2vhLgds63dZRyZlaHfJt3TmcEG9U06=s360-c-no"
      />
      <div style="display: flex; justify-content: space-between; gap: 2rem; align-items: center">
        <hr />
        <a class="link" href="https://github.com/devinbroussard" target="_blank">
          <button class="icon-btn">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="50"
              height="50"
              fill="currentColor"
              class="bi bi-github"
              viewBox="0 0 16 16"
            >
              <path
                d="M8 0C3.58 0 0 3.58 0 8c0 3.54 2.29 6.53 5.47 7.59.4.07.55-.17.55-.38 0-.19-.01-.82-.01-1.49-2.01.37-2.53-.49-2.69-.94-.09-.23-.48-.94-.82-1.13-.28-.15-.68-.52-.01-.53.63-.01 1.08.58 1.23.82.72 1.21 1.87.87 2.33.66.07-.52.28-.87.51-1.07-1.78-.2-3.64-.89-3.64-3.95 0-.87.31-1.59.82-2.15-.08-.2-.36-1.02.08-2.12 0 0 .67-.21 2.2.82.64-.18 1.32-.27 2-.27s1.36.09 2 .27c1.53-1.04 2.2-.82 2.2-.82.44 1.1.16 1.92.08 2.12.51.56.82 1.27.82 2.15 0 3.07-1.87 3.75-3.65 3.95.29.25.54.73.54 1.48 0 1.07-.01 1.93-.01 2.2 0 .21.15.46.55.38A8.01 8.01 0 0 0 16 8c0-4.42-3.58-8-8-8"
              />
            </svg></button
        ></a>
        <a class="link" href="https://github.com/devinbroussard" target="_blank">
          <button class="icon-btn">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="50"
              height="50"
              fill="currentColor"
              class="bi bi-linkedin"
              viewBox="0 0 16 16"
            >
              <path
                d="M0 1.146C0 .513.526 0 1.175 0h13.65C15.474 0 16 .513 16 1.146v13.708c0 .633-.526 1.146-1.175 1.146H1.175C.526 16 0 15.487 0 14.854zm4.943 12.248V6.169H2.542v7.225zm-1.2-8.212c.837 0 1.358-.554 1.358-1.248-.015-.709-.52-1.248-1.342-1.248S2.4 3.226 2.4 3.934c0 .694.521 1.248 1.327 1.248zm4.908 8.212V9.359c0-.216.016-.432.08-.586.173-.431.568-.878 1.232-.878.869 0 1.216.662 1.216 1.634v3.865h2.401V9.25c0-2.22-1.184-3.252-2.764-3.252-1.274 0-1.845.7-2.165 1.193v.025h-.016l.016-.025V6.169h-2.4c.03.678 0 7.225 0 7.225z"
              />
            </svg></button
        ></a>
        <a href="https://mail.google.com/mail/?view=cm&fs=1&to=devin.ray.broussard@gmail.com"
          ><button class="icon-btn">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="50"
              height="50"
              fill="currentColor"
              class="bi bi-send-fill"
              viewBox="0 0 16 16"
            >
              <path
                d="M15.964.686a.5.5 0 0 0-.65-.65L.767 5.855H.766l-.452.18a.5.5 0 0 0-.082.887l.41.26.001.002 4.995 3.178 3.178 4.995.002.002.26.41a.5.5 0 0 0 .886-.083zm-1.833 1.89L6.637 10.07l-.215-.338a.5.5 0 0 0-.154-.154l-.338-.215 7.494-7.494 1.178-.471z"
              />
            </svg></button
        ></a>
        <hr />
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
  width: 450px;

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
  text-shadow: 0 0 10px blue;
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

.nav-icons {
  color: white;
  position: absolute;
  width: 100%;
  display: flex;
  justify-content: end;
  gap: 1rem;
  padding-right: 2rem;
  padding-left: 2rem;
  padding-top: 2rem;
}

.about-img {
  border-radius: 100%;
  width: 10rem;
  border: 2px solid white;
  margin-bottom: 1rem;
}

.link:hover {
  background-color: none !important;
}

a:hover {
  background-color: none;
}

hr {
  border: 1px solid lightgray;
  width: 25px; 
  border-radius: 30px;
}
</style>
