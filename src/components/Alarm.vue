<script>
import AlarmItem from './AlarmItem.vue';
import AlarmModal from './AlarmModal.vue';

export default {
  data() {
    return {
      alarms: [],
      isModalOpen: true,
    };
  },
  components: {
    AlarmItem,
    AlarmModal,
  },
  mounted() {
    this.alarms = JSON.parse(localStorage.getItem('vue-alarm')) || this.alarms;
  },
  methods: {
    toggleAlarm: function (id, isActive) {
      this.alarms[id].isActive = isActive;
      localStorage.setItem('vue-alarm', JSON.stringify(this.alarms));
    },
    closeModal: function () {
      this.isModalOpen = false;
    },
    openModal: function () {
      this.isModalOpen = true;
    },
  },
};
</script>

<template>
  <div class="alarm">
    <div class="header">
      <h1>Alarms</h1>
      <button @click="openModal">+</button>
    </div>
    <AlarmModal :isOpen="isModalOpen" @close="closeModal" />
    <AlarmItem
      v-for="(alarm, index) in alarms"
      :key="index"
      :alarmId="index"
      :alarm="alarm"
      @toggle="toggleAlarm"
    />
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
</style>
