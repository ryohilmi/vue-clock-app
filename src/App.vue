<script setup>
import Clock from './components/Clock.vue';
</script>

<template>
  <Clock />
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto+Mono:wght@300;400;500&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html {
  background: rgb(15, 20, 30);
  color: white;
  font-family: 'Roboto Mono';
}

.clock {
  margin: 0 auto;
}
</style>
