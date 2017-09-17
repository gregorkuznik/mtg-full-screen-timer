<template>
  <div class="main-container">
    <div class="title-container">
      <input v-model="title">
    </div>
    
    <div class="timer-container"
      v-bind:class="{
        red: started && !reversed && duration < 60,
        green: started && reversed
      }">

      <span class="sign" v-if="started && reversed">-</span>
      <input class="input-timer minutes"
        :disabled="started"
        :value="minutes | padNumber"
        @input="updateValue('minutes', $event.target.value)">
      <span class="separator">:</span>
      <input class="input-timer seconds"
        :disabled="started"
        :value="seconds | padNumber"
        @input="updateValue('seconds', $event.target.value)">

    </div>
    
    <div class="buttons-container"
      v-bind:class="{
        hidden: started && !paused,
      }">
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
      title: 'Edit title',
      duration: 0,
      minutes: 50,
      seconds: 0,
      paused: true,
      started: false,
      reversed: false,
      interval: undefined,
      resetMinutes: 50,
      resetSeconds: 0
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
      this.resetMinutes = this.minutes;
      this.resetSeconds = this.seconds;
      this.calculateDuration();
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
      this.reversed = false;
      this.started = false;
      this.paused = true;
      this.stopInterval();
      this.minutes = this.resetMinutes;
      this.seconds = this.resetSeconds;
      this.duration = 0;
    },
    calculateDuration: function () {
      this.duration = (this.minutes * 60) + this.seconds;
    },
    setTime: function (duration) {
      this.minutes = Math.floor(duration / 60) % 60;
      this.seconds = duration % 60;
    },
    startInterval: function () {
      this.interval = setInterval(() => {
        if (this.duration <= 0) {
          this.reversed = true;
        }

        if (!this.reversed) {
          this.duration -= 1;
        } else {
          this.duration += 1;
        }

        this.setTime(this.duration);
      }, 1000);
    },
    stopInterval: function () {
      window.clearInterval(this.interval);
    },
    updateValue: function (field, value) {
      var formattedValue = Number(value);

      if (isNaN(formattedValue) || !Number.isInteger(formattedValue)) {
        formattedValue = this[field];
      } else if (formattedValue > 59) {
        formattedValue = this[field];
      } else if (formattedValue < 0) {
        formattedValue = this[field];
      }

      this[field] = formattedValue;
      this.$forceUpdate();
    }
  }
}
</script>

<style scoped>
  .main-container {
    width: 100vw;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }
  .title-container {
    display: flex;
    justify-content: center;
    align-items: flex-end;
    flex-shrink: 0;
    width: 100%;
    flex-grow: 1;
    margin: 0;
    z-index: 1;
  }
  .title-container > input {
    width: 100%;
    font-size: 5vw;
    text-align: center;
    border: none;
    background: transparent;
  }
  .timer-container {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 35vw;
    font-size: 34vw;
    cursor: default;
    color: black;
  }
  .timer-container.red {
    color: red;
  }
  .timer-container.green {
    color: green;
  }
  .input-timer {
    font-size: inherit;
    width: 38vw;
    height: 30vw;
    border: none;
    background: transparent;
  }
  .input-timer:disabled {
    color: inherit;
  }
  /*.input-timer.seconds {
    text-align: left;
  }
  .input-timer.minutes {
    text-align: right;
  }*/
  .sign {
    margin-top: -5vw;
    user-select: none;
  }
  .separator {
    margin-top: -2vw;
    user-select: none;
  }
  .buttons-container {
    display: flex;
    justify-content: center;
    align-items: flex-start;
    width: 100%;
    flex-grow: 1;
    z-index: 1;
  }
  .buttons-container.hidden > button {
    opacity: 0;
    pointer-events: none;
  }
  .buttons-container:hover > button {
    opacity: 1;
    pointer-events: all;
  }
  .buttons-container > button {
    font-size: 3vw;
    padding: 1.2vw 3vw;
    margin: 0 0.5vw 0 0.5vw;
    border-radius: 0.5vw;
    border: none;
    color: white;
    background-color: cornflowerblue;
    cursor: pointer;
    transition: opacity 0.2s ease-in-out;
    text-transform: uppercase;
  }
</style>
