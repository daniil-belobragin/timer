<template>
  <div class="timer">
    <custom-button :path="require('../assets/image/add.svg')" image-alt="add" :click-function="clickFunction" />
  </div>
</template>

<script>
import CustomButton from "@/components/CustomButton"

export default {
  name: "AddTimerButton",

  components: {CustomButton},

  props: {
    clickFunction: {
      required: true
    }
  }
}
</script>

<style lang="scss">
  @import "../assets/style/style";
</style>