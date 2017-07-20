<template>
  <div class="main-container">
    <h1>
      <input v-model="title">
    </h1>
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
      title: 'Vue.js full screen timer app',
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
    width: 100vw;
    height: 100vh;
    display: flex;
    justify-content: space-between;
    align-items: center;
    flex-direction: column;
  }
  h1 {
    position: fixed;
    width: 100%;
    top: 0;
    margin: 0;
  }
  h1 > input {
    width: 100%;
    font-size: 5vw;
    text-align: center;
    border: none;
    background: transparent;
  }
  .time {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 100%;
    font-size: 21vw;
    cursor: default;
  }
  .buttons {
    position: fixed;
    bottom: 0;
    margin-bottom: 2vh;
  }
  .buttons > button {
    font-size: 3vw;
    padding: 0.5vw 1.5vw;
    border-radius: 1vw;
    border: 0.1vw solid #e6e6e6;
    background-color: transparent;
    cursor: pointer;
  }
</style>
