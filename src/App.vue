<script setup>
import Clock from './components/Clock.vue';
</script>

<template>
  <Clock />
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Open+Sans:wght@300;400;500&family=PT+Serif:wght@400;700&family=Poppins:wght@100;200;300;400;500;600;700&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html {
  background: rgb(15, 20, 30);
  color: white;
  font-family: 'Poppins';
}

.clock {
  margin: 0 auto;
}
</style>
