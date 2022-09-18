<script>
export default {
  data() {
    return {
      time: {
        hours: '',
        minute: '',
        second: '',
      },
      date: 'Senin, 12 September 2022',
      timer: null,
    };
  },
  methods: {
    updateTime() {
      let options = {
        weekday: 'long',
        year: 'numeric',
        month: 'long',
        day: 'numeric',
      };
      let date = new Date();

      this.date = date.toLocaleDateString('id-ID', options);
      this.time = {
        hours: date.getHours().toString().padStart(2, '0'),
        minutes: date.getMinutes().toString().padStart(2, '0'),
        seconds: date.getSeconds().toString().padStart(2, '0'),
      };
    },
  },
  mounted() {
    this.timer = setInterval(() => {
      this.updateTime();
    }, 1000);
  },
  beforeDestroy() {
    clearInterval(this.timer);
  },
};
</script>

<template>
  <div class="clock">
    <p class="time">
      <span>{{ time.hours }}</span>
      <span class="semicolon">:</span>
      <span> {{ time.minutes }}</span>
      <span class="semicolon">:</span>
      <span>{{ time.seconds }}</span>
    </p>
    <p class="date">{{ date }}</p>
  </div>
</template>

<style lang="scss" scoped>
.clock {
  text-align: center;
  padding-top: 15em;

  .time {
    font-family: 'Roboto Mono';
    font-size: 5rem;
    font-weight: 500;
    font-variant-numeric: tabular-nums;
    color: rgb(57, 92, 105);
    text-align: center;
    text-shadow: rgba(192, 192, 192, 0.5) 2px 2px 7px;

    .semicolon {
      font-size: 0.8em;
      margin: 0 -5px;
      padding: 0;
      text-align: center;
    }
  }

  .date {
    font-weight: 400;
    color: rgb(114, 114, 114);
  }
}
</style>
