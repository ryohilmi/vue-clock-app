<script>
export default {
  data() {
    return {
      time: '19:00:00',
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
      let timestamp = new Date();

      this.time = timestamp.toLocaleTimeString('id-ID');
      this.time = this.time.replaceAll('.', ':');
      this.date = timestamp.toLocaleDateString('id-ID', options);
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
    <p class="time">{{ time }}</p>
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
  }

  .date {
    font-weight: 300;
    color: rgb(192, 192, 192);
  }
}
</style>
