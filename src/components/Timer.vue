<template>
  <div class="timer">
    <div class="time-wrapper" v-bind:class="{active: !isPaused && isStarted}">
      <span class="hours" v-show="hours !== '00'"> {{ hours }}: </span>
      <span class="minutes" v-show="minutes !== '00'"> {{ minutes }}: </span>
      <span class="seconds"> {{ seconds }} </span>
    </div>
    <div class="menu-wrapper" v-bind:class="{active: !isPaused && isStarted}">
      <custom-button :click-function="startTimer" :path="require('../assets/image/play.svg')" image-alt="play" v-show="isPaused || !isStarted" />
      <custom-button :click-function="pauseTimer" :path="require('../assets/image/pause.svg')" image-alt="pause"  v-show="!isPaused && isStarted"/>
      <custom-button :click-function="stopTimer" :path="require('../assets/image/stop.svg')" image-alt="stop" />
    </div>
    <button v-on:click="cloneTimer" class="clone-button">Clone</button>
  </div>
</template>

<script>
import CustomButton from "@/components/CustomButton";
export default {
  name: "Timer",
  components: {CustomButton},
  data () {
    return {
      seconds: 0,
      minutes: 0,
      hours: 0,
      isPaused: false,
      timer: null,
      remainTime: this.time
    }
  },
  props: {
    time: {
      required: true
    },
    onCloneTimer: {
      type: Function,
      required: true
    },
    onStopTimer: {
      type: Function,
      required: true
    }
  },

  computed: {
    isStarted() {
      return this.timer !== null && this.timer !== undefined
    }
  },

  methods: {
    setTime () {
      this.seconds = this.remainTime%60
      this.minutes = Math.floor(this.remainTime/60)%60
      this.hours = Math.floor(this.remainTime/60/60)%60

      this.seconds < 10 ? this.seconds = '0' + this.seconds: ''
      this.minutes < 10 ? this.minutes = '0' + this.minutes: ''
      this.hours < 10 ? this.hours = '0' + this.hours: ''
    },
    startTimer () {
      this.isPaused = false

      this.timer = setInterval(() => {
        if (!this.isPaused) {
          this.remainTime -= 1
          this.setTime()
        }
        if (this.remainTime <= 0) {
          this.stopTimer()
          this.remainTime = 0
          this.setTime()
        }
      }, 1000)
    },
    pauseTimer () {
      this.timer = clearInterval(this.timer)
      this.isPaused = true
    },
    cloneTimer () {
      this.onCloneTimer(this.remainTime)
    },
    stopTimer () {
      this.timer = clearInterval(this.timer)
      this.onStopTimer()
    }
  },
  mounted() {
    this.setTime()
  }
}
</script>

<style lang="scss">
  @import "../assets/style/style";

  .time-wrapper {
    height: 50%;
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    border-bottom: 1px solid #9E9E9E;
  }
  .menu-wrapper {
    height: 50%;
    width: 68px;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  .active {
    filter: brightness(200%);
  }

  .clone-button {
    color: white;
    background: none;
    border: none;
    outline: none;
    padding: 8px;
  }
</style>