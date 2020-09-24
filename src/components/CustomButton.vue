<template>
  <button class="button" v-on:click="clickFunction">
    <img class="button-image" :src="path" :alt="imageAlt">
  </button>
</template>

<script>
export default {
  name: "CustomButton",

  props: {
    path: {
      required: true
    },
    imageAlt: {
      required: true
    },
    clickFunction: {
      required: true
    }
  }
}
</script>

<style lang="scss">
  @import "../assets/style/style";
</style>