<script setup>
import Clock from './components/Clock.vue';
import Alarm from './components/Alarm.vue';
</script>

<template>
  <Clock />
  <Alarm />
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto+Mono:wght@300;400;500&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@100;300;400&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html {
  background: rgb(248, 250, 251);
  color: rgb(57, 92, 105);
  font-family: 'Roboto Mono';
}

.clock {
  margin: 0 auto;
}
</style>
