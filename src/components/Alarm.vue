<script>
import AlarmItem from "./AlarmItem.vue";

export default {
  data() {
    return {
      alarms: [
        { hour: 7, minute: 0, isActive: false },
        { hour: 7, minute: 0, isActive: true },
      ],
    };
  },
  components: {
    AlarmItem,
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
