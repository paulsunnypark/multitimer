<template>
  <div class="app">
    <button @click="addTimer">Create Timer</button>
    <button @click="closeAllTimers" v-if="timers.length > 1">Close Timer</button>
    <button @click="startAllTimers" v-if="timers.length > 1">Start All</button>
    <button @click="endAllTimers" v-if="timers.length > 1">End All</button>
    <RaceTimer v-for="(timer, index) in timers" :key="index" :timerId="index" @remove="removeTimer" ref="timerRefs"/>
  </div>
</template>

<script>
import RaceTimer from './components/RaceTimer.vue'

export default {
  components: {
    RaceTimer
  },
  data() {
    return {
      timers: []
    }
  },
  methods: {
    addTimer() {
      this.timers.push({})
    },
    closeAllTimers() {
      this.timers = this.timers.filter((timer, index) => this.$refs.timerRefs[index].isRunning)
    },
    removeTimer(timerId) {
      this.timers.splice(timerId, 1)
    },
    startAllTimers() {
      this.$refs.timerRefs.forEach((timer) => {
        if (!timer.isRunning) timer.startTimer();
      });
    },
    endAllTimers() {
      this.$refs.timerRefs.forEach((timer) => {
        if (timer.isRunning) timer.endTimer();
      });
    },
  }
}
</script>

<style>
.app {
  width: 80%;
  margin: 0 auto;
  border: 2px solid blue;
  overflow-y: auto;
  height: 90vh;
  padding: 10px;
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap; 
}

.app button {
  width: 23%; /* buttons to take up ~1/4 of the row, with a little space for margin */
  margin: 1%;
  padding: 5px;
  font-size: 1.0em; /* Adjust this value to control the size of the button text, not exceeding 14px */
  max-height: 50px; /* Controls the maximum height of the buttons */
}

.race-timer {
  border-bottom: 2px solid #000;
  margin-bottom: 20px;
  padding-bottom: 20px;
  padding: 5px;
  width: 100%;
}

.race-timer button {
  display: inline-block; /* Change this to inline-block to have buttons in the same row */
  width: 48%;
  margin-bottom: 5px;
  padding: 5px;
  font-size: 1.0em; /* Adjust this value to control the size of the button text */
}
</style>
