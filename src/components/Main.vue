<template>
  <div class="main-container">
    <h1>Vue.js full screen timer app</h1>
    <div class="time">{{ hours | padNumber }}:{{ minutes | padNumber }}:{{ seconds | padNumber }}</div>
    <div class="buttons">
      <button v-if="!started" v-on:click="startTimer">Start</button>
      <button v-if="started && !paused" v-on:click="pauseTimer">Pause</button>
      <button v-if="started && paused" v-on:click="resumeTimer">Resume</button>
      <button v-if="started" v-on:click="resetTimer">Reset</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'main',
  data () {
    return {
      duration: 0,
      hours: 0,
      minutes: 0,
      seconds: 0,
      paused: true,
      started: false,
      interval: undefined
    }
  },
  filters: {
    padNumber: function (value) {
      if (value < 10) {
        return '0' + value;
      } else {
        return value;
      }
    }
  },
  methods: {
    startTimer: function () {
      this.started = true;
      this.paused = false;
      this.startInterval();
    },
    pauseTimer: function () {
      this.paused = true;
      this.stopInterval();
    },
    resumeTimer: function () {
      this.paused = false;
      this.startInterval();
    },
    resetTimer: function () {
      this.started = false;
      this.paused = true;
      this.stopInterval();
      this.hours = 0;
      this.minutes = 0;
      this.seconds = 0;
      this.duration = 0;
    },
    setTime: function (duration) {
      this.hours = Math.floor(duration / 3600) % 60;
      this.minutes = Math.floor(duration / 60) % 60;
      this.seconds = duration % 60;
    },
    startInterval: function () {
      this.interval = setInterval(() => {
        this.duration = this.duration + 1;
        this.setTime(this.duration);
      }, 1000);
    },
    stopInterval: function () {
      window.clearInterval(this.interval);
    }
  }
}
</script>

<style scoped>
  .main-container {
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }
  h1 {
    font-size: 5vw;
  }
  .time {
    width: 96%;
    font-size: 20vw;
    text-align: center;
    border: 0.5vw solid #e6e6e6;
    border-radius: 50vw;
  }
  .buttons > button {
    margin-top: 1em;
    font-size: 3vw;
    padding: 1vw 4vw;
    border-radius: 50vw;
    border: 2px solid #e6e6e6;
    background-color: #f0f0f0;
  }
</style>
