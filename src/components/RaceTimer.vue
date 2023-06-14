<template>
  <div class="race-timer">
    <h2>{{ timer }}</h2>
    <button @click="startTimer" v-if="startButtonText !== 'Details View'">{{ startButtonText }}</button>
    <button @click="endTimer">{{ endButtonText }}</button>
    <button @click="toggleDetails" v-if="endButtonText === 'Reset'">{{ detailsButtonText }}</button>
    <div v-if="showLapDetails">
      <h3>Lap Details:</h3>
      <ul>
        <li v-for="(lap, index) in laps" :key="index">
          Lap {{ lap.lap }}: {{ lap.time }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isRunning: false,
      timer: '00:00:00',
      startButtonText: 'Start',
      endButtonText: 'End',
      detailsButtonText: 'Details View',
      intervalId: null,
      laps: [],
      lapCount: 0,
      showLapDetails: false,
    }
  },
  methods: {
    startTimer() {
      if (this.startButtonText === 'Start') {
        this.isRunning = true;
        this.startButtonText = `Lap Record ${this.lapCount}`;
        this.intervalId = setInterval(() => {
          let parts = this.timer.split(':').map(part => parseInt(part, 10));
          parts[2]++;
          if (parts[2] >= 60) {
            parts[1]++;
            parts[2] = 0;
          }
          if (parts[1] >= 60) {
            parts[0]++;
            parts[1] = 0;
          }
          this.timer = parts.map(part => part.toString().padStart(2, '0')).join(':');
        }, 1000);
      } else if (this.isRunning) {
        this.lapCount++;
        this.laps.push({
          lap: this.lapCount,
          time: this.timer,
        });
        this.startButtonText = `Lap Record ${this.lapCount}`;
        this.showLapDetails = false;
      }
    },
    endTimer() {
      if (this.endButtonText === 'End') {
        this.isRunning = false;
        clearInterval(this.intervalId);
        this.intervalId = null;
        this.startButtonText = 'Details View';
        this.endButtonText = 'Reset';
      } else {
        this.timer = '00:00:00';
        this.startButtonText = 'Start';
        this.endButtonText = 'End';
        this.laps = [];
        this.lapCount = 0;
        this.showLapDetails = false;
      }
    },
    toggleDetails() {
      if (this.showLapDetails) {
        this.showLapDetails = false;
        this.detailsButtonText = 'Details View';
      } else {
        this.showLapDetails = true;
        this.detailsButtonText = 'Hide Details';
      }
    },
  },
}
</script>

<style scoped>
.race-timer {
  border-bottom: 2px solid #000;
  margin-bottom: 20px;
  padding-bottom: 20px;
}
</style>
