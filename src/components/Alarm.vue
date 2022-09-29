<script>
import AlarmItem from "./AlarmItem.vue";

export default {
  data() {
    return {
      alarms: [],
    };
  },
  components: {
    AlarmItem,
  },
  mounted() {
    this.alarms = JSON.parse(localStorage.getItem("vue-alarm")) || this.alarms;
  },
  methods: {
    toggleAlarm: function (id, isActive) {
      this.alarms[id].isActive = isActive;
      localStorage.setItem("vue-alarm", JSON.stringify(this.alarms));
    },
  },
};
</script>

<template>
  <div class="alarm">
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
  min-width: 300px;
  max-width: 50%;
  margin: 4rem auto;
  padding: 1rem;
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
