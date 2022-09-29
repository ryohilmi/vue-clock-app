<script>
import AlarmItem from './AlarmItem.vue';
import AlarmModal from './AlarmModal.vue';

export default {
  data() {
    return {
      alarms: [],
      isModalOpen: false,
      alarmSound: new Audio('alarm.mp3'),
      timer: null,
      isPlaying: false,
      hasPlayed: false,
      currentAlarm: null,
      selectedAlarm: null,
      isEditing: false,
    };
  },
  components: {
    AlarmItem,
    AlarmModal,
  },
  mounted() {
    this.alarms = JSON.parse(localStorage.getItem('vue-alarm')) || this.alarms;
    this.timer = setInterval(() => {
      this.checkAlarms();
    }, 1000);
    document.addEventListener('mousemove', this.onMouseMove);
  },
  beforeDestroy() {
    clearInterval(this.timer);
  },
  watch: {
    alarms: {
      handler() {
        localStorage.setItem('vue-alarm', JSON.stringify(this.alarms));
      },
      deep: true,
    },
  },
  methods: {
    checkAlarms: function () {
      let date = new Date();

      this.date = date.toLocaleDateString('id-ID');

      const hours = date.getHours();
      const minutes = date.getMinutes();

      this.alarms.forEach((alarm, i) => {
        if (
          alarm.hour === hours &&
          alarm.minute === minutes &&
          this.hasPlayed === false &&
          alarm.isActive
        ) {
          this.currentAlarm = i;
          this.isPlaying = true;
          this.hasPlayed = true;
          this.$emit('playAlarm');
        }
      });
    },
    onMouseMove: function () {
      if (this.isPlaying) {
        this.alarmSound.play();
      }
    },
    stopAlarm: function () {
      this.isPlaying = false;
      this.alarmSound.pause();
      this.alarmSound.currentTime = 0;
      this.$emit('stopAlarm');
    },
    addAlarm: function (hour, minute, name) {
      this.alarms.push({
        hour,
        minute,
        name,
        isActive: true,
      });
    },
    editAlarm: function (index, hour, minute, name) {
      this.alarms[index] = { hour, minute, name, isActive: true };
    },
    removeAlarm: function (index) {
      this.alarms.splice(index, 1);
    },
    toggleAlarm: function (id, isActive) {
      this.alarms[id].isActive = isActive;
    },
    closeModal: function () {
      this.isModalOpen = false;
    },
    openModal: function (alarm, index) {
      if (index !== undefined) {
        this.selectedAlarm = alarm;
        this.selectedAlarm.index = index;
        this.isEditing = true;
      } else {
        this.isEditing = false;
      }
      this.isModalOpen = true;
    },
  },
};
</script>

<template>
  <div class="alarm" @mousemove="onMouseMove">
    <div class="header">
      <h1>Alarms</h1>
      <button @click="openModal">+</button>
    </div>
    <AlarmModal
      :isOpen="isModalOpen"
      @close="closeModal"
      @add="addAlarm"
      @edit="editAlarm"
      @delete="removeAlarm"
      :alarm="selectedAlarm"
      :isEditing="isEditing"
    />
    <AlarmItem
      v-for="(alarm, index) in alarms"
      :key="index"
      :alarmId="index"
      :alarm="alarm"
      @toggle="toggleAlarm"
      @click.self="openModal(alarm, index)"
    />
    <div class="alarm-warning" :class="{ flex: isPlaying }">
      <p>{{ alarms[currentAlarm]?.name || 'Alarm is Playing' }}</p>
      <button @click="stopAlarm">STOP</button>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.alarm {
  font-family: 'Roboto';
  min-width: 300px;
  max-width: 50%;
  margin: 4rem auto;
  padding: 1rem;
  display: flex;
  flex-direction: column;
  align-items: center;

  .header {
    display: flex;
    justify-content: space-between;
    width: 100%;
    margin: 1rem auto;

    h1 {
      font-weight: 300;
    }

    button {
      cursor: pointer;
      outline: none;
      border: none;
      width: 40px;
      height: 40px;
      font-size: 1.5rem;
      background: rgb(101, 99, 253);
      color: white;
      border-radius: 12px;
      box-shadow: -4px -2px 4px 0px rgba(231, 231, 231, 0.1),
        4px 2px 6px 0px rgba(228, 228, 247, 0.1);

      &:hover {
        filter: brightness(85%);
      }
    }
  }
}

.flex {
  display: flex !important;
}

.alarm-warning {
  display: none;
  position: fixed;
  top: 50px;
  background: white;
  width: 200px;
  height: 50px;
  border-radius: 10px;
  flex-direction: column;
  text-align: center;
  justify-content: center;
  padding: 2rem;
  z-index: 11;
  box-shadow: -4px -2px 4px 0px #ffffff,
    4px 2px 6px 0px rgba(209, 217, 230, 0.9);

  button {
    cursor: pointer;
    outline: none;
    border: none;
    width: 100px;
    padding: 0.3rem;
    margin: 0.3rem 0;
    align-self: center;
    background: rgb(101, 99, 253);
    color: white;
    border-radius: 12px;
    box-shadow: -4px -2px 4px 0px rgba(231, 231, 231, 0.1),
      4px 2px 6px 0px rgba(228, 228, 247, 0.1);

    &:hover {
      filter: brightness(85%);
    }
  }
}
</style>
