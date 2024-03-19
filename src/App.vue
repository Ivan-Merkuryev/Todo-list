<template>
  <header>
    <hello-world />
  </header>
</template>

<script setup>
import HelloWorld from "./components/Home.vue";
</script>

<style lang="sass">
*
  font-family: 'Nunito Sans', sans-serif
body
  box-sizing: border-box
  font-family: 'Nunito Sans', sans-serif
  margin: 0
  max-height: 100%
// #app
//   background: linear-gradient(#FFC5F6, #F997CF)
//   width: 100%
//   height: 100%
ul, li
  list-style-type: none
  margin: 0
  padding: 0
p, span
  font-size: 16px
input, button
  cursor: pointer
</style>
