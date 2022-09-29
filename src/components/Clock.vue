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

      const hours = ((date.getHours() + 11) % 12) + 1;
      const minutes = date.getMinutes();
      const seconds = date.getSeconds();

      const hour = hours * 30;
      const minute = minutes * 6;
      const second = seconds * 6;

      document.querySelector('.hour').style.transform = `rotate(${hour}deg)`;
      document.querySelector(
        '.minute'
      ).style.transform = `rotate(${minute}deg)`;
      document.querySelector(
        '.second'
      ).style.transform = `rotate(${second}deg)`;
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
    <div class="object">
      <span class="second"></span>
      <span class="hour"></span>
      <span class="minute"></span>
      <span class="marker six"></span>
      <span class="marker three"></span>
      <span class="marker nine"></span>
      <span class="marker twelve"></span>
    </div>
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
  padding-top: 3rem;

  .object {
    margin: 2rem auto;
    width: 300px;
    height: 300px;
    border-radius: 100%;
    overflow: hidden;
    position: relative;
    align-items: center;
    background-color: rgb(248, 250, 251);
    box-shadow: 8px 8px 12px 0 rgba(0, 0, 0, 0.1),
      -8px -8px 12px 0 rgba(255, 255, 255, 0.15);
    background: linear-gradient(
      135deg,
      rgba(0, 0, 0, 0.03),
      rgba(255, 255, 255, 0.025)
    );

    .minute,
    .hour {
      position: absolute;
      height: 60px;
      width: 5px;
      margin: auto;
      top: -60px;
      left: 0;
      bottom: 0;
      right: 0;
      border-radius: 4px 4px 8px 8px;
      background: rgb(101, 99, 253);
      transform-origin: bottom center;
      transform: rotate(0deg);
      z-index: 1;
    }

    .minute {
      position: absolute;
      height: 80px;
      width: 5px;
      top: -80px;
      transform: rotate(200deg);
    }

    .second {
      position: absolute;
      height: 105px;
      width: 3px;
      margin: auto;
      top: -105px;
      left: 0;
      bottom: 0;
      right: 0;
      border-radius: 4px;
      background: #d8d8d8;
      box-shadow: 8px 8px 12px 0 rgba(0, 0, 0, 0.1),
        -8px -8px 12px 0 rgba(255, 255, 255, 0.15);
      transform-origin: bottom center;
      transform: rotate(180deg);
      z-index: 1;
    }

    .marker {
      position: absolute;
      height: 15px;
      width: 15px;
      margin: auto;
      border-radius: 100%;
      transform-origin: bottom center;
      transform: rotate(0);
      z-index: 1;
      background-color: rgb(209, 209, 209);
    }

    .twelve {
      top: 2.5%;
      left: 47.5%;
    }

    .six {
      top: 92.5%;
      left: 47.5%;
    }

    .three {
      top: 45%;
      right: 15px;
      transform: rotate(90deg);
    }

    .nine {
      top: 45%;
      left: 15px;
      transform: rotate(270deg);
    }
  }

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
    font-family: 'Roboto';
    font-weight: 400;
    color: rgb(114, 114, 114);
  }
}
</style>
