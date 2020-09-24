<template>
  <div id="app">
    <transition-group tag="ul" name="list" class="wrapper">
      <li v-for="timer in timers" :key="timer.id" class="list-item">
        <timer :time="timer.startTime" :on-clone-timer="cloneTimer" :on-stop-timer="onStopTimer.bind(this, timer)" v-if="!timer.isAddButton"/>

        <add-timer-button :click-function="addTimer" v-else/>
      </li>
    </transition-group>
  </div>
</template>

<script>
import {Timer as TimerModel} from "./util"
import Timer from "@/components/Timer"
import AddTimerButton from "@/components/AddTimerButton";

export default {
  name: 'App',

  data () {
    return {
      timers: [new TimerModel(0, true)]
    }
  },

  components: {
    AddTimerButton,
    Timer
  },

  methods: {
    addTimer () {
      let startTime = Math.floor(Math.random() * 7200)
      let timer = new TimerModel(startTime, false)
      this.timers.splice(this.timers.length - 1, 0, timer)
    },
    cloneTimer (startTime) {
      let timer = new TimerModel(startTime, false)
      this.timers.splice(this.timers.length - 1, 0, timer)
    },
    onStopTimer (timer) {
      if (window.confirm('Really want to remove timer?')) {
        this.timers = this.timers.filter(obj => obj.id !== timer.id)
      }
    }
  }
}
</script>

<style lang="scss">
  @import "./assets/style/style";

  .wrapper {
    display: grid;
    justify-content: center;
    grid-template-columns: repeat(auto-fit, minmax(275px, 1fr));
    width: 100%;
    padding: 72px 50px;
    box-sizing: border-box;
  }
</style>
